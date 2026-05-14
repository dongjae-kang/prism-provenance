# JSON Data Contract — PRISM Vector Pipeline

**Lock 일시**: 2026-05-13
**용도**: 재훈 backend가 frontend로 serve할 JSON 구조. 본 contract가 양쪽 lock.

---

## A. Top-level

```json
{
  "schema_version": "1.0",
  "generated_at": "2026-05-13T12:46:00Z",
  "anthropic_call_mode": "live",
  "trend": { ... },
  "videos": [ ... ],
  "comments_inline": [ ... ]
}
```

NDJSON 권장 (1 line = 1 trend).

---

## B. Trend object

```json
{
  "trend_id": "trend_2026-05-13_chumiae-yang-hyangja",
  "topic_label": "추미애 경기지사 후보 + 양향자 국민의힘 후보 확정 맞대결",
  "issue_category": "선거",
  "n_days_tracked": 1,
  "origin": {
    "type": "external_event",
    "label": "국민의힘 경기지사 후보 양향자 선출 (정당 회의)",
    "estimated_time": "2026-05-02T01:00:00Z",
    "canonical_text": null
  },
  "anchor_video_id": "t3CpY3skB0g",
  "video_count": 8,
  "outlet_count": 5,
  "amplification_total_views": 19917
}
```

`origin.type`: `"external_event"` | `"canonical_text"` | `"first_video_fallback"`.

---

## C. Video record

```json
{
  "video_id": "mpzT2Hv6VM4",
  "url": "https://www.youtube.com/watch?v=mpzT2Hv6VM4",
  "title": "[조응천 인터뷰] \"당신들이 윤석열 만든거야!\" ...",
  "channel": {
    "id": "UCwRljhjVWtLqAKbsWGPU_OA",
    "title": "YTN 라디오",
    "authority_tier": 2
  },
  "published_at": "2026-05-13T11:09:04Z",
  "duration_seconds": 1157,
  "is_shorts": false,
  "view_count": 6856,
  "like_count": 605,
  "comment_count": 137,
  "thumbnail_url": "https://i.ytimg.com/vi/mpzT2Hv6VM4/hqdefault.jpg",
  "thumbnail_description": "Haiku 4.5 Vision 100자 description...",
  "description_excerpt": "■ 이슈인터뷰 ...",
  "has_captions": true,
  "captions_kind": "asr",
  "transcript_chars": 9234,
  "transcript_useful": true,
  "transcript_summary": "Haiku 4.5 200 token 요약 (entity 정정 포함)...",
  "embedding": {
    "model": "voyage-3-large",
    "dimension": 1024,
    "vector": [/* 1024 floats */],
    "input_chars": 860,
    "input_tokens": null
  },
  "axes": {
    "axis_authority": 0.8,
    "axis_time_distance": 0.002,
    "axis_semantic_drift": 0.345
  },
  "axis_raw": {
    "authority_tier": 2,
    "hours_since_origin": 0.25,
    "cosine_with_anchor": 0.655
  },
  "diffusion": {
    "is_anchor": false,
    "preceded_by": [
      { "video_id": "...", "overlap": 7 }
    ]
  }
}
```

---

## D. Axis 계산식 (재현 가능)

```
# X: authority — (6 - tier) / 5
T1 → 1.0, T2 → 0.8, T3 → 0.6, T4 → 0.4, T5 → 0.2, unknown → 0.0

# Y: time distance — (hours_since_origin / 168) clip [0, 1]
1 주일 정규화. > 1주면 1.0 clip.

# Z: semantic drift — 1 - cosine(video_embedding, anchor_embedding)
0 = 동일, 1 = orthogonal.
```

**Anchor 정책 (admin page dropdown)**:
- (a) `earliest`: 트렌드 첫 영상 (Track 1.5 default)
- (b) `highest_authority`: 권위 가장 높은 영상 (메이저 매체 origin)
- (c) `curator_pick`: 큐레이터 manual

---

## E. Comments inline (별도 layer, §4.8용)

```json
{
  "comments_inline": [
    {
      "video_id": "mpzT2Hv6VM4",
      "author": "ryu2343ryu",
      "text": "선거를 지려고 하는 당은 처음 본다 ㅋㅋ",
      "like_count": 36,
      "published_at": "2026-05-02T15:24:00Z",
      "curator_picked": false
    }
  ]
}
```

`comments_inline`은 sample 1셋 작성에서 미충족 (commentThreads.list 별도 호출 필요). Step 1.5 후속.

---

## F. Authority tier mapping (Track 1.5 seed)

| Tier | 정의 | 예시 |
|---|---|---|
| T1 (1.0) | 정치인·정당·정부 공식 | (sample에 없음) |
| T2 (0.8) | 주요 언론 verified | YTN, MBC, KBS, SBS, JTBC, 조선일보, 중앙일보, 동아일보, 한겨레, 경향신문, 연합뉴스 |
| T3 (0.6) | 중견 매체 / 정치 전문 | (운영 중 추가) |
| T4 (0.4) | 정치 YouTuber / 정치 전문 | 정치뉴스(빨간맛파란맛) |
| T5 (0.2) | shorts / 익명 / 미분류 | 진태양난티비, 북극곰티비 |

운영 중 정아인+Jack manual labeling 누적.

---

## G. DB schema 권장사항 (재훈 선택권 보존)

### 옵션 1: 단순 JSON file (Track 1.5 launch 권장)
- `/data/trends/YYYY-MM-DD/trend_{id}.json` per trend
- 디버그 쉬움, version control 가능, frontend 직접 fetch 가능

### 옵션 2: SQLite + sqlite-vec
- file-based, zero-deploy
- vector similarity SQL 가능
- write concurrency 제한 — 큐레이터 동시 작업 시 주의

### 옵션 3: PostgreSQL + pgvector (트래픽 증가 시)
- vector similarity + 일반 SQL 쿼리
- hosting 부담

**Claude 권장**: 옵션 1 또는 2 for Track 1.5. 5/30 launch 후 트래픽 증가 시 옵션 3 migration.

재훈 최종 결정권.

---

## H. Sample 파일

`sample/trend_sample.json` — 위 schema 전부 fill된 실데이터.
`sample/max_disperse_sample.json` — 매체 3시각 선정 결과.

---

*Schema LOCK. 변경 시 schema_version bump + migration note.*
