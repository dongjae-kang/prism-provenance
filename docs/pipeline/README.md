# PRISM 벡터 파이프라인 — 재훈 Backend Integration

**작성**: 2026-05-13 (Jack × Claude Code, 벡터 파이프라인 deep work 세션)
**대상**: [[김재훈]] backend developer
**관련 콜**: 2026-05-13 PRISM 팀 콜 1h 03m (`~/vault/sources/2026-05-13-prism-jaehoon-call.md`)
**Status**: 6단계 + Step 7 max-disperse 모두 lock. Sample 1셋 full fill 검증 완료.

---

## 0. 빠른 entry (재훈이 5분 안에 파악할 것)

1. **본 폴더 파일 4개 + sample 2개** = backend integration 전 spec.
2. **Sample data 파일 2개**가 핵심 — 이게 production에서 frontend로 흘러갈 JSON contract.
   - [`sample/trend_sample.json`](./sample/trend_sample.json) — 1 trend × 8 video full record (embedding 1024-dim + 3축 + 메타 + Haiku vision/요약)
   - [`sample/max_disperse_sample.json`](./sample/max_disperse_sample.json) — 매체 3시각 선정 결과
3. **재훈 D-day 시연 zero-friction**: sample 파일 frontend에서 fetch → 카드 1장 render. 백엔드 endpoint 없어도 시연 가능.

---

## 1. 파이프라인 6단계 + Step 7 요약

| Step | 책임 | 비용 |
|---|---|---|
| 1. YouTube 데이터 추출 | YT API + youtube_transcript_api v1 instance API | quota 602/일 |
| 2. Feature engineering | 단일 string 결합 (제목+설명+썸네일desc+자막요약) + 메타 분리 | $0 |
| 3. 임베딩 (Voyage voyage-3-large) | dim 1024, batch API | free tier 200M tokens |
| 4. 3축 normalize | X=권위, Y=시간거리, Z=의미변형 | $0 |
| 5. Schema (JSON contract) | 본 폴더 `schema.md` | — |
| 6. Sample fill | Haiku vision + 요약 + Voyage batch | $0.026 / 8 video |
| 7. 매체 3시각 max-disperse | brute force C(unique_channels, 3) maximize sum pairwise (1-cosine) | <50ms realtime |

세부: [`pipeline-stages.md`](./pipeline-stages.md)

---

## 2. JSON 데이터 contract

[`schema.md`](./schema.md) — 카드당 record 구조. 본 contract가 frontend·backend 양쪽 lock된 형태.

핵심:
```
{
  trend: { trend_id, topic_label, origin, anchor_video_id, video_count, outlet_count, ... },
  videos: [
    {
      video_id, title, channel: {id, title, authority_tier},
      published_at, view_count, like_count, comment_count,
      thumbnail_url, thumbnail_description,
      transcript_summary,
      embedding: { model, dimension, vector[1024] },
      axes: { axis_authority, axis_time_distance, axis_semantic_drift },
      diffusion: { is_anchor, preceded_by[] }
    },
    ...
  ],
  comments_inline: []  // §4.8 흐름+반응 inline, Step 1.5 후속 추가
}
```

---

## 3. 책임 분담 (5/13 콜 lock)

### 재훈 (backend)
- Production batch cron 운영 — daily 5 trend × N video pipeline 실행
- Admin page (어드민 페이지) — 큐레이터 검수 + 공개/비공개 toggle
- Backend endpoint `/api/trends/{id}.json` serve
- DB schema 결정권 (옵션 권장: SQLite + sqlite-vec for Track 1.5)

### Jack (Producer)
- Dev·실험·sample 1셋 작성 책임
- 큐레이션 lead (PRD §6.1 일일 워크플로우)
- 비용·API key 발급 + monitoring
- 디자인·voice·콘텐츠 최종 결정

→ **재훈도 API key 보유** (production cron 실행). [`keys-ownership.md`](./keys-ownership.md) 참조.

---

## 4. 재훈 D-day 작업 흐름 (KST 5/15 저녁 또는 5/16)

1. **본 README 5분 read**
2. **`sample/trend_sample.json` 검사** — 어떤 데이터가 들어오는지 익숙해지기
3. **Frontend card 1장 render** — sample data 그대로 fetch → 카드 1장 표시 (axes·channel·thumbnail·summary 모두 보이게)
4. **3D scatter (선택)** — `pipeline-stages.md` §Step 7에 Plotly.js prototype 골격. v8 hover overlay 통합 또는 별도 page.
5. **Admin page wireframe** — Jack이 별도 spec 카톡으로 전달 예정 (5/14)
6. **Jack과 시연 미팅** — 카드 render + 3D scatter + admin page sketch 시연

---

## 5. 산출물 파일 (이 폴더)

```
docs/pipeline/
├── README.md                       이 파일 (5분 entry)
├── pipeline-stages.md              6단계 + Step 7 deep dive
├── schema.md                       JSON contract spec
├── keys-ownership.md               API key ownership + 수령 path (값은 별도 카톡)
└── sample/
    ├── trend_sample.json            8 video full record
    └── max_disperse_sample.json     매체 3시각 선정 결과
```

---

## 6. 비용 envelope

- **Sample 1셋 (8 video)**: $0.026 (Haiku 10 calls + Voyage 1 batch)
- **Track 1.5 20일 launch 운영** (5 trend × 50 video pool): **$17-18** (Anthropic $20 envelope fit)
- **Voyage embedding**: $0 (free tier 200M tokens, 3-4년치)

비용 mitigation lever (운영 중 즉시 적용):
1. 자막 input cap 8K → 2K (-$4.5)
2. Haiku 4.5 → Haiku 3.5 (-$3-4)
3. Discovery 모수 50 → 25 video/trend (-$9)

---

## 7. 알려진 한계 (정직 disclosure)

| 한계 | 영향 | 완화 |
|---|---|---|
| Anchor가 earliest time 기반 | 큰 매체 origin 아닐 수 있음 | admin page에서 (a/b/c) override |
| 자막 useful ≥ 300자는 ~37% | 자막 의존도 낮춤 | 제목·설명·썸네일이 baseline |
| ASR entity 오인식 ("추미해", "조흥천") | 자막 raw embedding 시 노이즈 | Haiku 요약 단계 entity 정정 prompt 작동 |
| 5-gram overlap false positive (같은 channel template) | Diffusion edges 일부 부정확 | 같은 channel_id 제외 + threshold 상향 + LLM verify (Track 2) |
| Haiku Vision 한국어 OCR text 정확도 | 썸네일 text 일부 오인식 | Visual framing은 정확 — embedding 시그널 충분 |
| Authority tier mapping seed 작음 | 처음 보는 채널 → T5 default | 운영 중 정아인+Jack manual labeling 누적 |

---

## 8. References

- PRD v4 Core: `../../PRD-v4-core.md`
- Data Sourcing Plan: `../data-sourcing-plan-0512.md`
- 5/13 PRISM 콜 source note: `~/vault/sources/2026-05-13-prism-jaehoon-call.md`
- Working dir (Jack 환경, repo 외): `~/Desktop/prism-pipeline-0513/`

---

*질문은 Jack에게 카톡.*
