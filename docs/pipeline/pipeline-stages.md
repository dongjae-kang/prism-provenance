# Pipeline Stages — 6단계 + Step 7 Deep Dive

**Lock 일시**: 2026-05-13
**검증 source**: Sample 1셋 (walkthrough V1~V8 재현, 8 video full fill)

---

## Step 1: YouTube 데이터 추출

### Endpoints
- `search.list` (100 quota / call) — Discovery cascade
- `videos.list` (1 quota, batch up to 50 IDs) — meta + statistics
- `captions.list` (1 quota / video) — 자막 트랙 존재 확인
- `channels.list` (1 quota, batch) — 채널 메타
- `commentThreads.list` (1 quota / page) — Top-N 댓글 (relevance order)

### 자막 fetch
- **NOT** `captions.download` (OAuth + owner-only 제약, 200 quota)
- **YES** `youtube_transcript_api` v1.x instance API:
  ```python
  ytt = YouTubeTranscriptApi()
  fetched = ytt.fetch(video_id, languages=["ko", "ko-KR"])
  text = " ".join(s.text for s in fetched)
  ```

### 실측 자막 가용률 (한국 정치)
- 트랙 존재: ~62.5% (모두 ASR auto-generated)
- Useful ≥ 300자: ~37%
- 없음: ~37%
→ 자막은 *boost layer*, 베이스라인은 제목+설명+썸네일.

### Quota envelope
- 1일 운영 (5 trend × 50 video pool): ~602 units / 10,000 daily quota (6%)

---

## Step 2: Feature Engineering

### 텍스트 결합 (단일 vector path)

```python
parts = [
    f"[제목] {title}",
    f"[채널] {channel_title}",
    f"[설명] {description[:500]}",
    f"[썸네일] {thumbnail_haiku_description}",
]
if transcript_chars >= 300:
    parts.append(f"[자막요약] {transcript_haiku_summary}")
embedding_input = "\n".join(parts)
```

**길이**: ~860자 (자막 있음) / ~660자 (자막 없음). Voyage 32K token limit 안.

### 메타 분리
- axis scalar (axis_authority·time·drift) → 별도 컬럼
- 카드 attribute (view·like·comment·channel·duration) → meta 객체

### 관계 feature
Track 1.5 skip. 5-gram phrase overlap만 (Step 7 diffusion).

---

## Step 3: Embedding Model

**Voyage voyage-3-large** lock.

| Spec | 값 |
|---|---|
| Dimension | 1024 |
| Multilingual + Korean | 강세 (2026 MTEB ko 상위) |
| Batch API | max 128 inputs / request |
| Free tier | 200M tokens (3-4년치) |
| Latency | ~200ms |
| Rate limit (payment method 등록 시) | 300 RPM |

### Batch call pattern
```python
import voyageai
vo = voyageai.Client(api_key=os.environ["VOYAGE_API_KEY"])
result = vo.embed(
    [embedding_input_1, embedding_input_2, ...],   # all videos in 1 trend
    model="voyage-3-large",
    input_type="document"
)
embeddings = result.embeddings   # list of 1024-dim vectors
```

### Fallback chain
1차 Voyage → 2차 OpenAI text-embedding-3-large → 3차 KoSimCSE local

---

## Step 4: 3-Axis Normalize

| Axis | 공식 | 의미 |
|---|---|---|
| `axis_authority` (X) | (6 - tier) / 5 | 분광 — 매체 권위 |
| `axis_time_distance` (Y) | min(hours_since_origin / 168, 1.0) | 흐름 — 시간 거리 |
| `axis_semantic_drift` (Z) | 1 - cosine(video_emb, anchor_emb) | 분광 — 의미 변형도 |

PRD §1.3 두 frame (프리즘 + 흐름) 모두 cover. v8 hero radial spread 시각화와 정합.

**대안 swap (운영 중 admin page toggle)**:
- Z → `axis_spread_rate` = log10(views / hours + 1) / log10(10000) — hook trajectory와 align

---

## Step 5: Schema

[`schema.md`](./schema.md) 참조.

---

## Step 6: Sample Fill (검증된 작업 flow)

### Phase 1 — Per-video enrichment
1. Thumbnail vision: Haiku 4.5 Vision call per video
   ```python
   resp = anthropic_client.messages.create(
       model="claude-haiku-4-5-20251001",
       max_tokens=180,
       messages=[{
           "role": "user",
           "content": [
               {"type": "image", "source": {"type": "base64", "media_type": "image/jpeg", "data": img_b64}},
               {"type": "text", "text": THUMBNAIL_PROMPT}
           ]
       }]
   )
   ```
2. Transcript summary (자막 ≥ 300자만): Haiku 4.5 entity 정정 prompt
   - "추미해 → 추미애" 정확 정정 검증됨
   - "조흥천 → 조응천" 정확 정정 검증됨
   - 광고·간투사·반복·화자 마커 제거

### Phase 2 — Batch embedding
모든 video의 결합 텍스트를 1 batch call로 Voyage:
```python
emb_resp = voyage_client.embed(
    [v["embedding_input"] for v in videos],
    model="voyage-3-large",
    input_type="document"
)
```

### Phase 3 — Axes 계산
- Anchor 선정 (earliest publishedAt, fallback policy)
- 각 video → axis_authority·time·drift 채움

### Phase 4 — Diffusion edges
5-gram char-level phrase overlap ≥ 3 → `preceded_by` edges.
**Caveat**: 같은 channel template 반복으로 false positive. Track 1.5에서는 GRAPH (weekly only) 노출.

### 비용 (8 video sample)
- Anthropic: $0.026 (Haiku 10 calls — vision 8 + summary 2)
- Voyage: $0 (batch 1 call, 2,285 tokens)

---

## Step 7: 매체 3시각 Max-Disperse

### 알고리즘
```python
# Input: N videos w/ 1024-dim embeddings + channel_id + axes
# Output: 3 selected video_ids (다른 channel, max sum pairwise distance)

# 1. Channel representative (1 per channel_id, highest authority → highest views)
# 2. Pairwise distance matrix (N × N): (1 - cosine)
# 3. Brute force C(N, 3) combinations:
#       score = sum of 3 pairwise distances
# 4. Return: combo with max score
```

### Complexity
- N=15 unique channels (production trend): C(15,3)=455 combos × 1024-dim cosine ≈ **50ms**
- Realtime 가능.

### Sample 결과 (sample data 그대로)
- 선정 3: 진태양난티비 + YTN 라디오 + 조선일보 (sum 1.16)
- Worst combo: 진태양난티비 + 정치뉴스 + 북극곰티비 (sum 0.70) — 모두 정치 YouTuber cluster, 알고리즘이 회피.

### 3D 시각화 (v8 hero hover overlay)
Plotly.js scatter3d. Single HTML prototype 골격:
```html
<script src="https://cdn.plot.ly/plotly-2.35.0.min.js"></script>
<div id="plot" style="width:100%;height:600px"></div>
<script>
fetch('/data/trends/today/trend_xxx.json').then(r => r.json()).then(d => {
  // 선정 3 video: marker size 15, saturated color
  // 나머지: size 6, gray, opacity 0.35
  // anchor: star marker
  // axes: X=권위, Y=시간거리, Z=의미변형
  Plotly.newPlot('plot', traces, layout);
});
</script>
```

### Anchor 정책 (admin page dropdown)
- (a) earliest publishedAt — Track 1.5 default
- (b) highest_authority — 메이저 매체 origin
- (c) curator_pick — admin page에서 manual

---

## Pipeline 운영 흐름 (KST 매일 08:30)

```
08:30  Discovery cascade (search.list 4 modes)
       → 후보 video pool 50-100개
       
08:45  videos.list + channels.list + captions.list (batch)
       → meta 채움
       
09:00  Per-video Haiku Vision (thumbnail) + 자막 fetch + Haiku 요약
       → 자막 useful video 약 1/3
       
09:30  Voyage batch embedding (모든 video in 1 call)
       → 1024-dim vectors
       
09:45  Axes 계산 + diffusion edges + max-disperse 선정
       → JSON file generation per trend
       
10:00  Admin page에 surface (재훈 backend)
       → 큐레이터 (Jack/정아인) review
       
11:00  Approve gate → publish
       → frontend serve
```

비용: ~$1/일 (Track 1.5 launch scale). $20 envelope 20일.

---

*Pipeline stages LOCK.*
