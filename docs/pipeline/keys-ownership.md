# API Key Ownership Map

**작성**: 2026-05-13
**원칙**: 키 *값*은 본 문서에 *없음*. Jack이 카톡으로 별도 secure channel 공유.

---

## A. 키 5종 — ownership·사용 위치

| Key | 발급자 | 보관자 (둘 다) | Production 호출 위치 | 사용 task |
|---|---|---|---|---|
| `YOUTUBE_API_KEY` | 김재훈 | Jack + 재훈 | 재훈 backend cron | YouTube Data API v3 — search·videos·captions·channels·commentThreads |
| `NEC_API_KEY` | 김재훈 | Jack + 재훈 | 재훈 backend cron | data.go.kr 선관위 권한 |
| `ANTHROPIC_API_KEY` | Jack | Jack + 재훈 | 재훈 backend cron (production) / Jack 환경 (dev) | Haiku 4.5 — 썸네일 vision + 자막 요약 |
| `VOYAGE_API_KEY` | Jack | Jack + 재훈 | 재훈 backend cron (production) / Jack 환경 (dev) | voyage-3-large embedding |
| `OPENAI_API_KEY` | (미발급) | — | — | 본 use case strictly necessary 아님 |

### 5/13 콜 lock 정정
이전 "Anthropic·Voyage = Jack-only" 표현은 *user request 시 realtime LLM call 안 함* 의도였음. *Batch precompute cron*은 재훈 backend가 운영. → 재훈도 키 보유.

---

## B. 키 값 수령 path (재훈 액션)

### 단계
1. **카톡으로 Jack이 전달** (2026-05-13 또는 이후) — secure channel
2. **재훈 본인 환경에 secure storage**:
   - `.env` file (mode 600, gitignored)
   - 또는 1Password / Bitwarden secure note
   - **절대 GitHub commit 금지** (public repo, secret scan risk)
3. **production cron에서 환경변수로 load**

### 절대 금지
- ❌ GitHub commit (이 폴더 포함 어디든)
- ❌ Slack public channel 평문
- ❌ 이메일 평문 (forward 가능성)
- ❌ public Gist / pastebin

---

## C. 운영 시 사용량 monitoring

| Service | 잔액·사용량 확인 | Frequency |
|---|---|---|
| Anthropic | console.anthropic.com → Plans & Billing → Current Balance | Daily (운영 중) |
| Voyage | dashboard.voyageai.com → Usage | Weekly (free tier 200M tokens, 3-4년 안전) |
| YouTube | console.cloud.google.com → APIs & Services → Quotas | Weekly |
| data.go.kr | data.go.kr 마이페이지 | Monthly |

### 비용 알람 (Track 1.5 운영 중)
- Anthropic 잔액 < $5 → Jack 즉시 충전
- 일일 비용 > $1.5 → mitigation lever 적용 (자막 input cap / Haiku 3.5 swap / 모수 절감)

---

## D. 키 rotation policy

- 본 세션에서 카톡 평문 전송된 키 (YouTube·NEC·Anthropic·Voyage) → Track 1.5 launch *전*에 1회 rotation 권장
- 6개월마다 정기 rotation (Q1, Q3)
- 외부 유출 의심 시 즉시 revoke + 재발급

각 service rotation path:
- Anthropic: console → API Keys → 기존 revoke + 새 create. 환경변수 update.
- Voyage: dashboard → API Keys → 동일.
- YouTube: Google Cloud Console → Credentials → API Key → Regenerate.

---

## E. 비용 분담 (현재 lock)

| Service | 결제 책임 | 사용량 책임 |
|---|---|---|
| Anthropic ($20 충전) | Jack | 양쪽 (dev + production) |
| Voyage (free tier) | Jack (계정 owner) | 양쪽 |
| YouTube (free quota) | 재훈 (GCP 계정) | 양쪽 |
| data.go.kr (무료) | — | 양쪽 |

Anthropic·Voyage paid charge는 현재 0 (Voyage free tier). 향후 paid 진입 시 Jack 부담. 5/30 launch 후 비용 정착 시점에 분담 재논의 가능.

---

## F. 환경변수 file template (재훈 backend)

```bash
# .env (mode 600, gitignored)
# Source: Jack 카톡 2026-05-13

YOUTUBE_API_KEY=AIza...
NEC_API_KEY=4f9c...
ANTHROPIC_API_KEY=sk-ant-api03-...
VOYAGE_API_KEY=pa-...
```

`.gitignore`에 `.env` 추가 필수.

---

*Key ownership map LOCK. 키 값 변경 시 카톡 + 본 문서 timestamp update.*
