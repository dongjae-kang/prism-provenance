# PRISM Provenance — PRD v4 Detailed (Reasoning Archive)

*같은 사실에 대해 매체별로 다른 시각이 어떻게 펼쳐지는지를 매일 보여주는 프리즘 매체*

**Status**: PRD v4 lock (2026-05-08)
**Companion**: [`PRD-v4-core.md`](./PRD-v4-core.md) — product spec (self-contained)
**Backward-compat reference**: [`PRD-v3.md`](./PRD-v3.md) — v3 reasoning archive (보존, 5/8 frame shift 이전 reasoning 참고용)

이 문서는 **product spec이 아니다** — spec은 v4-core. 이 문서는 v4 결정의 reasoning trail + 거부된 alternatives + risk archive + 5/8 design decisions의 detail이다. v3 detailed와 달리 spec을 중복 서술하지 않는다.

**v3 → v4 cascade 요약**: Frame shift "흐름 → 프리즘 매체"가 §1, §4.5 hook #3, §11 Differentiation, §12 Tech Stack을 동시에 바꿈. 동시에 5/8 디자인 논의가 IA 3탭, 흐름+반응 통합, 매체 3시각, 정정 두 종류, 검색 부활, 카드 콘텐츠 정의, voice 변경, anti-pattern lock 등 13+ 결정을 추가.

---

## 1. Frame Shift 분석 — "흐름 매체 → 프리즘 매체"

### 1.1 무엇이 바뀌었나

| Layer | v3 | v4 |
|---|---|---|
| 메타포 정체성 | 흐름 매체 (시간상 변형) | 프리즘 매체 (매체별 다각도) + 흐름 (시간) |
| Hero 메타포 | 5색 광선이 갈라지는 추상 일러스트 (SpectrumHero) | 같은 사실 → 매체별 다른 수치 분광 (현실 데이터) |
| Differentiation 한 줄 | "정보가 어떻게 변형되며 퍼졌나 (how it moved)" | "같은 사실을 매체가 어떻게 다르게 펼치나 (how it spreads + how it transforms)" |
| Mission verb | 추적합니다 | 따라가요 |
| Voice 톤 | newspaper / editorial | 친근 / 따라가요 |

### 1.2 왜 바뀌었나 (Transcript 인용)

**Jack 본인의 frame shift** [pt2 L2042-2045]:
> "필요한 건 나는 이게 흐름 매체여서 흐름을 보여주는 게 제일 중요하다는 생각을 했는데, 오늘 너랑 얘기를 해보니까 이 프리즘이 더 중요하긴 하네. 다양한 사실에 대해서 여러가지 계획하고 펴잖아요."

**해석**:
- v3 mental model: 사실(점) → 시간상 변형(선)
- v4 mental model: 사실(점) → 매체 다각도 분광(면) → 그 분광 안에서 시간 변형(선)
- 즉 흐름은 프리즘의 부분집합이 됨. 메타포 위계 변경.

**왜 이게 중요한가**:
1. **Hook layer hero design**의 출발점이 바뀜. v3 hero는 추상 광선 일러스트 (Jack 본인이 "최악" 비판). v4 hero는 오늘의 실제 데이터를 분광으로 보여줌 → 더 strong concrete signal.
2. **Differentiation framing**이 다 바뀜. v3 "한국에 흐름 매체 없음" → v4 "한국에 매체 다각도 분광 매체 없음 + AllSides는 있지만 라벨링 차이". 더 명확한 white space.
3. **매체 3시각 비교 (§4.7)**가 자연스러운 first-class feature가 됨. v3에서는 부수적, v4에서는 PRISM 정체성과 같은 axis.

### 1.3 Frame shift가 cascade한 섹션

**Direct cascade (필수 변경):**
- §1 한 줄 정의 — 메타포 자체 변경
- §4.5 hook #3 (radial spread hero) — 추상 → 현실 데이터 분광
- §4.5 hook #5 (hero lede) — "다섯 가지 트렌드 추적" → "오늘은 X가 N갈래로 흘러갔어요"
- §11 Differentiation — AllSides 비교 추가, "how it moved" → "how it spreads + transforms"
- §12 Tech Stack — 매체 3시각 cross-reference layer 추가

**Indirect cascade (디자인 영역):**
- Voice 톤 — 추적 → 따라가요 (Jack 데모 카피 전체 개정)
- Visual language — newspaper / editorial 색채 → modern / 친근
- Logo 메타포 — 5색 광선 분산 → 분광 (TBD Saetbyeol)

---

## 2. 5/8 Design Decisions — Detailed Reasoning

이 섹션은 v4-core §15 Design Decisions Log의 reasoning expansion. 각 결정의 (a) 무엇을 선택했나 (b) 거부된 alternatives (c) 근거 transcript (d) 후속 영향.

### 2.1 IA 3탭 (홈 / 오늘의 이슈 / 피드)

**선택**: 메인 nav 3개 — 홈 / 오늘의 이슈 / 피드. How We Work는 footer.

**거부된 alternatives:**
- **(a) 2탭 (홈 / 피드)** [pt2 L501] — Jack 첫 발화는 2탭이었음. 현미 pushback으로 3탭 전환. 근거: 홈은 entry point + spread overview, 오늘의 이슈는 daily 5장 deck, 피드는 카테고리 묶음 — 셋이 distinct.
- **(b) How We Work를 메인 nav 유지** — Jack v3 데모 패턴. 5/8 거부 근거: 메인 nav는 콘텐츠 진입 우선. 이용약관급 정보는 footer.
- **(c) 사이드 nav** — 모바일 friction 우려, 모바일 first 원칙과 충돌.

**근거** [pt2 L501-549]:
> Jack: "지금 소개가 없어지면 탭이 두개가 있는거잖아 / 홈 피드 두개만 있는거잖아"
> Jack: "두 번째 탭의 핵심은 / 오늘 다섯 개가 보여진다는 게 핵심인 것 같고"
> Jack: "내가 생각하는 두 번째 탭은 / 오늘의 다섯 개인 거고 / 세 번째 탭은 주제별 카드인 거지"

**후속 영향**:
- Jack 데모 `App` (demo.html L2686-2719) `page` state `"home" | "how"` 2개 → `"home" | "today" | "feed"` 3개 확장.
- 현미 데모 `index.html` 헤더 nav `홈 / 피드 / 소개` → `홈 / 오늘의 이슈 / 피드`.
- How We Work 페이지 자체는 유지, footer 링크로만 진입.

---

### 2.2 검색 부활 (형태소 단위 PRD lock)

**선택**: PRD에는 형태소 단위 검색 포함. 데모는 가벼운 client-side prototype.

**v3 결정 (5/6)**: 검색 MVP 제외. 근거 [pt2 L820-839]: 형태소 분석 세팅 비용 + 6/3까지 카드 100개라 카테고리+날짜로 탐색 가능.

**v4 reframe (5/8)**: Jack이 PRD-level에서 검색을 부활시켰음. 데모에서는 가벼운 prototype OK, 최종 product에는 형태소 단위 검색 필요. 근거:
- 한국어 정치 콘텐츠는 매체별·정치인별 명사형이 다양 ("청년 주거 / 청년 무주택 / 청년 주택 부족" 등 동일 의미 다양 형태). 단순 substring match 부족.
- 1년+ 운영 시 카드 1,000+ 누적 → 검색 필수.
- 운영 가치 / 구현 비용 trade-off 재평가 결과 포함.

**거부된 alternatives:**
- **(a) 검색 영구 제외** — v3 결정 그대로. 5/8 Jack reframe으로 거부.
- **(b) 단순 string 검색** — 한국어 형태소 한계로 부족.
- **(c) Elasticsearch full-text** — overkill, mecab-ko/KOMORAN/khaiii 같은 형태소 분석기 + 단순 인덱스 충분.

**후속 영향**:
- Backend (재훈): 형태소 분석기 선택 + 인덱스 빌드.
- 데모: client-side filter (제목 + 본문 substring + 이슈 라벨).
- UI: 홈 hero 하단 또는 sticky bar.

---

### 2.3 흐름+반응 통합 (별 탭 X)

**선택**: REACT를 별 탭에서 빼고 FLOW step 안에 inline 통합. 흐름 step 옆에 그 매체·영상 댓글 1-2개.

**거부된 alternatives:**
- **(a) v3 5탭 (FLOW/SPREAD/TRACE/REACT/GRAPH) 유지** — 정보 분리 명확하지만 사용자 friction (탭 클릭). 5/8 거부.
- **(b) REACT를 SPREAD에 통합** — 의미 mismatch (확산 vs 사용자 반응).
- **(c) REACT 제거** — 사용자 sentiment 신호 자체가 misinformation 분석에 가치 있음. 거부.

**근거** [pt2 L1969-1973]:
> 현미 제안: "이 흐름에다가 반응을 같이 넣는 건 어때요? KBS 뉴스 7이라고 하면은?"
> Jack 즉답: "오 좋은데? 너무 좋아×2"

**왜 강한 결정인가**:
- 현미가 본인이 디자이너 아니라고 명시했음에도 [pt2 L208], UX 결정에서 압도적이었다고 Jack이 평가 [for-hyunmi 13절].
- 흐름 매체 정체성과 사용자 sentiment를 분리하지 않고 같은 unit에 표시 = "한 매체에서 발화된 클레임 + 그 매체 시청자 반응" 한 unit으로 묶임. 의미 단위가 더 자연스러움.

**후속 영향**:
- Jack 데모 `OriginTrail` (demo.html L922-994) — react 통합 redesign.
- 현미 데모 `claim.html` — flow 섹션에 react 통합.
- 카드 5탭 → 4탭으로 축소: FLOW (반응 통합) / SPREAD / TRACE / GRAPH (weekly only).

---

### 2.4 흐름 step 최대 7개 (변곡점 위주)

**선택**: 흐름 step 최대 7개. 변곡점·전환점 위주로 큐레이션.

**거부된 alternatives:**
- **(a) 모든 영상 시간순 표시** — 60+ 영상 가능 [pt2 L1975-1985 인접]. 정보 과부하.
- **(b) 5개 제한** — 변곡점 표현 부족 가능성.
- **(c) 10개 이상** — 카드 unit 안에 들어가기 어려움.

**근거** [pt2 L1975-1985 인접]:
> Jack: "이재명 대통령 당선 같은 이슈는 60개 채널이 다룰 수 있는데, 60개 다 보여줄 순 없잖아"

**큐레이션 정책 (디자인 영역 X)**:
- MVP: 큐레이터 manual selection 100%.
- Post-MVP: 자동 신호 (시간 delta + 출처 grade 변화 + view 급증 + 댓글 sentiment shift) → 큐레이터 final 합치기.

**후속 영향**:
- 디자인: layout이 7개 자연스럽게 다룰 수 있는 형태 — vertical timeline + scroll OK.
- Approver checklist에 "흐름 7 step 변곡점 위주인지" 추가 (§6.2 #7).

---

### 2.5 매체 3시각 비교 (라벨 X)

**선택**: 같은 클레임을 다룬 매체 3곳 시각 병치. **라벨 없이 매체명만**.

**거부된 alternatives:**
- **(a) AllSides 외부 라벨 차용 (좌·중·우)** — Show, don't classify (§4.4) 위반.
- **(b) PRISM 자체 라벨링** — 명예훼손 risk + 원칙 충돌.
- **(c) 매체 3시각 자체 거부** — 프리즘 메타포의 핵심. 5/8 거부.

**근거** [for-hyunmi §5]:
> 옵션 3 (라벨 없이 매체명만 병치 + 시각만 비교)이 가장 PRISM 원칙에 맞을 듯.

**왜 strong**:
- 사용자 자율성 존중 (사용자가 자력 추론).
- 법적 risk 최소 (라벨이 곧 평가).
- AllSides와 차별화 (기존 prior art가 라벨 채택했는데 PRISM은 거부 = 차별화 axis).

**후속 영향**:
- 디자인: 카드 안 매체 3시각 layout (현미).
- Backend: 같은 클레임 cross-reference (재훈 검토 필요).
- Approver checklist에 "매체 3시각 라벨 위반 여부" 추가 (§6.2 #8).

---

### 2.6 카드 콘텐츠 정의 — 단순 발표성 클레임 제외

**선택**: 출처 1개에서 나온 단순 발표성 사실 (예: "양향자 국민의힘 경기지사 후보 확정")은 PRISM 영역 X.

**거부된 alternatives:**
- **(a) Jack v3 데모 양향자 카드 유지** — 단순 발표 = 다양한 시각 충돌 X = 프리즘 메타포 부적합. 5/8 거부.
- **(b) 발표성 클레임도 다루되 분광 단순화** — PRISM 영역 정의 흐려짐.

**근거** [pt2 L1011-1030]:
> 단순 사실 발표성 클레임 (예: "양향자 국민의힘 경기지사 후보 확정"): 출처 1개 (선관위 발표)에서 나온 명백한 팩트, 다양한 시각이 충돌하지 않음, PRISM의 본 영역이 아님.

**다루는 것 (정의 lock)**:
- 통계 변형 (35% → 50% → 60%)
- 공약 / 정책 수치 변형 (27만호 → 50만호)
- 다양한 매체·채널·정치인이 같은 사건을 다르게 framing

→ **현미 데모의 6개 클레임이 이 정의에 맞아** (청년 무주택, 최저임금, AI 노동 대체율, 대학 등록금, 검찰 개혁 처리율, 수도권 인구). 다 통계·숫자 변형 중심.

---

### 2.7 매체 다양성 (언론사 + 유튜버 + 정치인)

**선택**: 매체 3시각·평일 카드 모두 언론사 + 유튜버 + 정치인 채널 모두 포함.

**거부된 alternatives:**
- **(a) 유튜버만 — 5/8 현미 입장**: "언론사 포함하면 절반 이상이 언론사일 듯. 유튜버 controversial한 거에 집중하자."
- **(b) 언론사만** — 프리즘 메타포의 정치 YouTube 시장 미스인포 핵심 누락.

**근거** [pt2 L1671-1689]:
> Jack: "공신력 있는 매체이기 때문에 미스인포가 안 다룬다는 생각은 절대 안 해. MBC 뉴스도 안철수 의원 발언 보도하잖아. 빼는 게 오히려 더 인위적인 개입."

**후속 영향**:
- 시드 채널 40-60개 (좌·우 균형) + 7개사 정치 카테고리 RSS + 정치인 공식 채널 모두 포함.
- 어떤 비율로 큐레이션할지 = 큐레이터 영역 (Jack + 정아인 + 김현미).

---

### 2.8 카톡 OG 미리보기 (자동 생성)

**선택**: 카톡 공유 시 단순 텍스트 X. og:image 자동 생성 템플릿. 카드 trajectory 시각화 + 헤드라인.

**거부된 alternatives:**
- **(a) 단순 link preview (제목+썸네일)** — 카톡 한국인 1순위 분배 채널이라 hook 강력 필요.
- **(b) Manual og:image per card** — 운영 부담.

**근거** (5/8 재훈):
> "그 카톡 공유 같은거는 / 카톡에 딱 올렸을 때 예쁘게 나오는 형태로 복사가 되는거죠"

**왜 중요한가**:
- 미스인포메이션 해결 = 좋은 정보가 쉽게 퍼지는 것.
- 카톡 OG가 강력 = 자발적 분배 강화.

**후속 영향**:
- Backend (재훈): og:image 자동 생성 template + 큐레이터 input slot ("카톡 한 줄 hook").
- 디자인: og:image template visual (카드 trajectory + 헤드라인). 데모 단계에서 mockup.

---

### 2.9 정정 요청 두 종류

**선택**: 두 종류로 분기 — 기자에게 (외부) / PRISM에게 (자체).

**거부된 alternatives:**
- **(a) 단일 정정 요청 (v3 그대로)** — 의도 두 종류였으나 UI에 노출 안 됨. 5/8 보강.
- **(b) 기자 정정만** — PRISM 자체 카드 정정 채널 결여.

**근거** [pt1 L298-326]:
- 기자에게: "이 부분 더 취재해주세요" → 외부 매체 actionable.
- PRISM에게: "이 카드 다시 업데이트해주세요" → 큐레이터 카드 수정.

**UI 설계**:
- 1차 선택 modal ("기자에게 / PRISM에게") → 분기된 form.
- 또는 두 버튼 분리 (디자인 영역, 현미).

**후속 영향**:
- Jack 데모 `CorrectionFormModal` (demo.html L2536-2598) — 두 종류 분기 추가.
- 현미 데모 `claim.html` — 정정 요청 버튼 두 갈래로.

---

### 2.10 Voice 변경 (추적 → 따라가요)

**선택**: PRISM voice 톤을 "추적합니다" → "따라가요"로 전환. 현미 voice 채택.

**거부된 alternatives:**
- **(a) Jack v3 voice 유지** — Jack 본인이 [pt1 L143-147]에서 자기 비판: "한 줄기 빛이 다섯 색으로 갈라지면 이런 이상한 멘트도 넣고 싶은 생각이 전혀 없단 말이지 / 그래서 이런 거 다 빼고 / 그리고 다섯 가지 트렌드를 추적했으면 이것도 진짜 최악이고."
- **(b) 더 캐주얼 (반말 / 위트)** — §4.4 "Show, don't classify" 위반 가능성. 거부.

**근거**:
- 현미 voice가 Jack 의도와 더 가까움 (Jack 본인 인정).
- "따라가요" = 친근 + 사용자 자율성 (가르치는 톤 0).
- 표준 존댓말 유지하되 친근.

**후속 영향**:
- Jack 데모 카피 전체 개정 (Hero / Footer / Card subtitle / 모든 micro-copy).
- 단 현미 본인도 디자이너 X 인정 — 5/11 Saetbyeol 합류 후 visual + voice 같이 final lock.

---

### 2.11 카드 nested 가로 캐러셀 제거

**선택**: 카드 자체는 옆 swipe (deck), 카드 내부는 vertical stack + 위쪽 탭 클릭. 가로 nested swipe 제거.

**거부된 alternatives:**
- **(a) v3 ducal 가로 nested swipe 유지** — 5/8 쌤님 비판 [pt1 L121-131]: "카드 자체가 옆으로 넘어가는데 카드 안에 있는 새우 콘텐츠도 이게 또 가로로 넘어가는 구조니까 이게 좀 불편하다." 5/8 거부.

**후속 영향**:
- Jack 데모 `CardSpread` (demo.html L1503-1756) — vertical stack + tabs로 재설계.
- 현미 데모 `claim.html` — 이미 vertical stack + tab 패턴, carry-over.

---

### 2.12 Weekly Flow 시각 강화

**선택**: Weekly Flow 카드 시각 강화. 5/11 Saetbyeol 합류 후 final lock.

**현재 상태**: v3 weekly card는 텍스트 위주, Jack 본인이 약하다고 인정.

**근거** [pt1 L329-359 Jack]:
> "텍스트만 있으면 이거는 전혀 좋은 게 아니라 생각하고 / 좀 이걸 사람들이 어떻게 하면 흥미로워하고 / 그리고 그 사람들이 원하는 관심있어하는 정보를 / 줄 수 있을지는 잘 모르겠어. / 그런 건 좀 나도 도움을 받고 싶은데."

**Knowledge Graph는 weekly에서만**:
- 개별 카드 KG는 단일 주제 주변 노드라 의미 약함 [pt2 L1907-1917].
- Weekly에서 여러 이슈 묶을 때 KG 활용 — 이슈끼리 연결되는 인물·주제 시각화 의미 있음.

**후속 영향**:
- Jack 데모 `WeeklyFlowCard` (demo.html L2027-2090) — 시각 강화 redesign (Saetbyeol 후).
- Jack 데모 `MiniKG` (demo.html L1181-1250) — 카드에서 빼고 weekly로 이동.

---

### 2.13 Anti-pattern lock

**선택**: 5/8 anti-pattern 9개 lock + preferences 5개 lock.

**Anti-patterns:**
- 바이브 코딩 느낌 (Claude로 빨리 뽑은 사이트 톤) — Jack 가장 강하게 거부 [pt1 L29, L456-457]
- 챗GPT 스타일 / 라이브 위키
- 불꽃 이모티콘 / 일반 이모티콘 헤드라인 — 현미 명시 [pt2 L228-233]
- 글자만 가득한 카드 (정보 한눈에 X)
- 모든 네모가 똑같이 생긴 카드
- "한 줄기 빛이 다섯 색으로 갈라집니다" 같은 추상 비유 멘트
- "오늘 다섯 가지 트렌드를 추적했습니다" 같은 형식적 헤드라인
- True/False 라벨링 / 분류 톤
- 카드 가로 nested swipe

**Preferences:**
- Apple Music 앨범아트 스타일 3D 카드 넘기기 (PC만) [pt1 L7-12]
- 친근한 voice ("따라가요") — 현미 톤이 Jack 의도와 더 가까움
- 시각 요소 풍부 (썸네일, 이미지, 차트, 분광)
- 한눈에 정보가 들어오는 layout
- "Show, don't classify" 일관

**모순 처리** (5/8 미해결, §14 Open Question #8):
- 현미 데모 follow.html에 📮💬🔗 이모지 + detail-actions에 💬📷✏️🔥 이모지 — 본인이 anti-pattern으로 비판한 것과 충돌.
- 해결 방향: 이모지 빼고 SVG 아이콘 + 텍스트 라벨. 단 현미 final 결정 위임.

---

## 3. v3 Hook Layer 8개 vs v4 변경

| Hook | v3 → v4 변경 | 이유 |
|---|---|---|
| 1. trajectory 제목 | 유지 | 변경 없음 |
| 2. N일째 카운터 3-layer | 유지 | 변경 없음 |
| 3. radial spread hero | **변경** | 추상 SpectrumHero 폐기 → 현실 데이터 분광 |
| 4. 분광 메타포 design language | 유지 (조건부) | quality bar 통과 시 (Saetbyeol 후) |
| 5. hero lede | **변경** | "다섯 가지 트렌드 추적" 폐기 → "오늘은 X가 N갈래로" 패턴 |
| 6. 리더보드 | 유지 | 변경 없음 |
| 7. 카톡 한 줄 | **강화** | og:image 자동 생성 추가 |
| 8. 정정 요청 | **분기** | 두 종류 (기자 / PRISM) |

---

## 4. v3에 없던 신규 항목 (v4 신규)

### 4.1 매체 3시각 비교 (§4.7)
v3에는 없는 first-class feature. AllSides 영감, 라벨 X. PRISM 정체성 = 프리즘 분광 = 매체 3시각.

### 4.2 흐름+반응 통합 (§4.8)
v3 5탭 → v4 4탭. REACT를 FLOW에 흡수. 사용자 친화 + 의미 단위 정합.

### 4.3 검색 형태소 단위 (§4.9)
v3 결정 (MVP 제외) reframe. 형태소 단위 PRD lock + 데모 prototype.

### 4.4 IA 3탭 신규 정의 (§4.0)
v3에 없던 메인 nav 명시. 홈 / 오늘의 이슈 / 피드.

### 4.5 카드 콘텐츠 정의 lock (§4.1)
v3에 없던 negative definition. 단순 발표성 클레임 제외 명시.

### 4.6 Approver checklist 6 → 8 (§6.2)
신규 2개:
- #7 흐름 7 step 변곡점 위주
- #8 매체 3시각 라벨 위반 여부

### 4.7 흐름 7 step 큐레이션 정책 (§6.3)
v3에 없던 큐레이션 정책. 변곡점 위주, MVP manual.

### 4.8 What We Are Not + 광고 매체 (§7)
v3 5개 → v4 6개. 현미 추가.

### 4.9 Open Questions 명시 (§14)
v3에는 risks에만 있던 unresolved 항목을 dedicated section으로 분리.

### 4.10 Design Decisions Log (§15)
v3에는 없던 decisions audit trail. 5/8 16개 결정 transcript line refs와 함께.

---

## 5. Risk Archive (v3 carry-over + v4 추가)

### 5.1 Legal Risks (carry-over)

**공직선거법 82조의4** — 후보자 비방·허위사실 공표 금지. *판단·평가 진술*에 적용.
**공직선거법 250조** — 허위사실 공표 + 후보자 비방. *판단·평가 진술*에 적용.
**한국 명예훼손법 (형법 307조 등)** — *판단·평가 진술*에 적용.

**Mitigation**:
- §4.4 "Show, don't classify" — 모든 evaluative 라벨 거부.
- §4.7 매체 3시각 라벨 X — AllSides 외부 라벨도 거부.
- §6.2 Approver gate — 명예훼손 risk 평가 (#6).
- 한국 변호사 preliminary check (5/24~5/29).

### 5.2 Operational Risks

**Burn-out** — 6명 part-time, daily cadence + 토요일 weekly + 흐름 7 step manual + 매체 3시각 cross-reference = v4 부담 증가.

**Mitigation**:
- Approver rotation
- 일요일 휴식
- Election week 후 mandatory rest week
- Post-MVP 자동 신호 도입으로 단계적 절감

### 5.3 Product Risks

**8개 이슈 카테고리 MECE 부족 가능성** — 5/10 전 재검증.

**Process Transparency Disclosure 역효과** — "AI 자동화 못 한다" 신호로 받아들여질 가능성. 페이지 framing으로 mitigation.

**v4 신규 — 매체 3시각 매칭 자동화 한계** — 같은 클레임을 다른 framing으로 다룬 매체 식별이 LLM에 어려움. 큐레이터 manual fallback.

**v4 신규 — voice 일관성 risk** — Jack 데모 voice 전체 개정 + 5/11 Saetbyeol 합류 + 김현미 UX 리드 + Jack 큐레이션 = 3-4명 voice 영역. 일관성 lock 필요 (5/16 QA 전).

### 5.4 Frame Shift 자체의 risk

**Risk**: "프리즘 매체"라는 메타포가 한국어 사용자에게 즉시 직관적이지 않을 가능성.

**Mitigation**:
- 한 줄 정의에서 "다양한 매체가 어떻게 다른 각도로 다루는가" 평이하게 표현.
- Hero 시각화가 메타포 자체를 시각적으로 설명 (현실 데이터 분광).
- 메타포 단어 ("프리즘", "분광", "흐름") 사용 빈도 신중. 큰 hero에만 등장하고 카드 단위에서는 평이한 한국어.

---

## 6. Working Doc 인덱스 (5/8 디자인 논의)

5/8 working doc (`~/Desktop/prism-design-iteration/working-doc.md`) 의 핵심 라인 인덱스 압축본:

### Transcript pt1 (23:38, 461 segments) 핵심
- L1-3: 두 데모의 정체 (Jack 데모는 현미 데모의 후속 iteration)
- L7-12: Apple Music 앨범아트 3D 카드 = Jack 좋아함
- L29: Jack 자기 데모 "바이브 코딩 같다" 자평
- L121-131: 쌤님 비판 (nested carousel)
- L143-147: SpectrumHero + "다섯 가지" 헤드라인 = Jack 비판
- L156-162: 카드 안 vertical stack 제안
- L289: Jack 자기 데모 "참사" 자평
- L298-326: 카톡 공유 + 정정 요청 (두 종류) 의도
- L329-359: Weekly Flow 카드 시각 약함
- L364-410: How We Work 위치 + 사람 검수 명시 이유
- L446-457: 3D 모바일 caveat + 바이브 코딩 거부

### Transcript pt2 (2:12:03, 2,174 segments) 핵심
- L1-13: 워크플로우 합의 (Jack 정리 → 현미 디자인)
- L29-30 [00:15:00]: hallucination (chunk boundary, 무시)
- L208: 현미 "디자이너 아님" 명시
- L210-212: Jack "디자이너의 일을 부탁하면서 너를 힘들게 하고 싶지 않거든"
- L228-233: 바이브 코딩 / 이모티콘 anti-pattern
- L309-313: 검색 메인 X (현미)
- L314-336: 네이버 vs 구글
- L329-336: 콘텐츠 플랫폼 vs 검색엔진 (현미 우세)
- L499-502: ★ 메인 nav 3탭 결정 (홈/오늘의 이슈/피드)
- L506-513: How We Work footer로 격하
- L605-624: 5개 한눈에 vs 디테일 (미해결)
- L750-753: 재훈 임팩트 있는 hero 결정 필요
- L820-839: 검색 MVP 제외 결정 (5/6) → 5/8 reframe (PRD lock)
- L864-874: 로그인 X 결정
- L917 [00:59:59]: hallucination (chunk boundary, 무시)
- L1011-1030: 양향자 케이스 = PRISM 영역 정의 (단순 발표성 제외)
- L1671-1689: 언론사 매체 포함 결정 (Jack 우세)
- L1907-1917: KG는 weekly에 적합
- L1923-1940: 댓글 프레이밍 효과 우려 (미해결)
- L1949-1957: AllSides 영감 매체 3시각
- L1969-1973: ★ 흐름+반응 통합 결정 ("오 좋은데? 너무 좋아×2")
- L1975-1985: 흐름 7개 step 제한
- L1989-2001: layered depth (MVP 제외)
- L2024-2030: 매체 3시각 디자인 ON, 백엔드 확인
- L2042-2045: ★ 흐름 매체 → 프리즘 매체 frame shift
- L2156-2158: 5/9 저녁 검토 약속

---

## 7. v3 → v4 코드 좌표 변경 (Jack 데모)

`~/github/prism-provenance/docs/demo.html` 기준. 데모 v4 빌드 시 변경 영향:

| 컴포넌트 | 라인 (v3) | v4 변경 |
|---|---|---|
| `:root` 변수 | L17-57 | 디자인 토큰 carry-over (5/11 Saetbyeol 후 visual lock) |
| MOCK DATA | L411-675 | 양향자 카드 제외, 단순 발표성 콘텐츠 교체 |
| `CARD_YANGHYANGJA` | L439-506 | **제거** |
| `SpectrumMark` | L682-721 | 로고 carry-over, 미세 조정 |
| `SpectrumHero` | L743-854 | **제거 또는 재설계** — 현실 데이터 분광 hero로 (현미 spread SVG 기반) |
| `NDayCounter` | L856-868 | carry-over |
| `OriginTrail` | L922-994 | **흐름+반응 통합** — react inline 추가 |
| `DiffusionCurve` | L1000-1081 | carry-over |
| `MiniKG` | L1181-1250 | **카드에서 제거 → weekly로 이동** |
| `HorizontalSnap` | L1258-1349 | nested carousel 변경 시 영향 |
| `CardSpread` | L1503-1756 | **vertical stack + tabs로 재설계** (5탭 → 4탭) |
| `HeroCard` | L1866-1944 | **헤드라인 + Hero illustration 재설계** (lede 패턴 + spread SVG) |
| `TrendCard` | L1950-2021 | nested 캐러셀 변경 따라 영향 + 매체 3시각 신규 슬롯 |
| `WeeklyFlowCard` | L2027-2090 | **시각 강화** (5/11 Saetbyeol 후 final) |
| `Deck` | L2096-2308 | 외곽 3D peek carry-over |
| `HowWeWork` | L2314-2517 | 페이지 carry-over, **메인 nav에서 footer로** |
| `CorrectionFormModal` | L2536-2598 | **두 종류 정정 분기 추가** |
| `Header` | L2613-2637 | **메인 nav 3탭으로 변경** |
| `Footer` | L2639-2680 | **How We Work 링크 추가** + voice 톤 변경 |
| `App` | L2686-2719 | **page state 3종으로 확장** (`"home" \| "today" \| "feed"`) |

**현미 데모 변경** (`~/Downloads/prism-demo/`):
- `index.html`: nav 3탭 변경, hero spread SVG carry-over (v4 hero base 후보)
- `feed.html`: nav 동기화
- `claim.html`: 흐름+반응 통합, 매체 3시각 신규, 정정 요청 두 종류 분기
- `about.html`: nav에서 빼고 footer 링크. 페이지 자체 carry-over.
- `follow.html`: carry-over (이메일/카톡 채널/RSS, 로그인 없는 구독)
- `styles.css`: 디자인 토큰 carry-over (5/11+ 통합)

---

## 8. v4 → 데모 v4 빌드 워크플로우

**선택된 빌드 전략 (5/8 lock)**: Jack 데모 base + 현미 패턴 React로 import (병합 v3).

**근거**:
- Jack 데모 React architecture + design tokens + 5-layer paper depth shadow + SpectrumMark logo + NDayCounter + DiffusionCurve SVG + MiniKG + CorrectionFormModal + WeeklyFlowCard + Pretendard+Source Serif+Mono typography 시스템 = 자산.
- 현미 데모 IA + voice + claim.html vertical stack pattern + feed.html category pattern + follow.html 구독 + hero spread SVG = 패턴.
- 자산 + 패턴 합집합이 가장 quality 보존 + 5/8 결정 모두 반영.

**빌드 surgery 단위 (~7-8 React component touch):**
1. `App` page state 확장 — 3탭 (홈/오늘의 이슈/피드)
2. `SpectrumHero` 제거 + 새 `SpreadHero` 컴포넌트 (현미 spread SVG React화)
3. `Header` nav 3탭으로
4. `OriginTrail` — 흐름+반응 통합 (react inline)
5. `CardSpread` — vertical stack + tabs (5탭 → 4탭)
6. 신규 `MediaSpectrum` 컴포넌트 (매체 3시각)
7. `CorrectionFormModal` — 두 종류 분기
8. `WeeklyFlowCard` — 시각 강화 (5/11+ final)

**voice 개정**: Hero / Footer / Card subtitle / micro-copy 전체 — "추적합니다" → "따라가요".

**검색 prototype**: 홈 hero 하단 sticky bar — client-side filter (제목 + 본문 substring + 이슈 라벨).

**데모 v4 빌드 위치**: `~/github/prism-provenance/docs/demo-v4.html` (v3 demo.html은 reference로 보존).

---

## 9. v4 → v5 예상 변경 (재훈 회신 대기)

5/8 Jack 약속 — 재훈에게 추가 확인:
1. **매체 3시각 백엔드 가능성** — 같은 클레임 cross-reference 자동 매칭 feasibility
2. **카톡 OG image 자동 생성** — 동적 generation infra
3. **흐름 7 step 큐레이션 자동 신호** — 변곡점 detection (시간 + 출처 grade + view + sentiment)

재훈 회신 후:
- 가능 → Track 1.5 포함, v5 patch
- 부분 가능 → MVP에 manual fallback, post-MVP 자동화
- 불가능 → MVP에서 제외, post-MVP roadmap

---

## Document Metadata

**Version**: v4.0 Detailed (Reasoning Archive — not self-contained spec)
**Last updated**: 2026-05-08 (NYC)
**Authors**: Jack Kang + Claude (writing assist, all decisions Jack-led, 5/8 김현미 협업)
**Companion**:
- [`PRD-v4-core.md`](./PRD-v4-core.md) — product spec (self-contained, ~25KB, 5분 onboarding read)
- [`PRD-v3.md`](./PRD-v3.md) — v3 reasoning archive (보존)

**Source decisions (chronological):**
- 2026-04-19 PLAN.md (Track 1.5 plan)
- 2026-05-05 [[김재훈]] 콜 — data.go.kr / 팩트체크 X 명시 / web 빌드
- 2026-05-06 hook layer brainstorm + timeline lock + 협업 흐름
- 2026-05-08 [[김현미]] 디자인 iteration (2h 35m, 13+ decisions, frame shift, anti-pattern lock)

**Working doc**: `~/Desktop/prism-design-iteration/working-doc.md` (50KB, 732 lines, transcript line refs)
**For-hyunmi**: `~/Desktop/prism-design-iteration/for-hyunmi.md` (16KB, 321 lines, 현미 검토용 정리본)

---

*End of PRD v4 Detailed*
