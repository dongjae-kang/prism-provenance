# PRISM Provenance — PRD v4 Core

*같은 사실에 대해 매체별로 다른 시각이 어떻게 펼쳐지는지를 매일 보여주는 프리즘 매체*

**Status**: PRD v4 lock (2026-05-08, frame shift "흐름 → 프리즘 매체" + 5/8 design decisions integrated)
**Demo hard-deadline**: 2026-05-09 ~ 05-10 (디자인 + hook 통합 작동 데모)
**Wrap-up**: 2026-05-16 ~ 05-17 (QA + final polish)
**Launch target**: 2026-05-30 (선거 D-4)
**First application**: 2026-06-03 지방선거 (Track 1.5)

**워크플로우:**
```
PRD v4 ─→ Demo v4 (5/8~10) ─→ Frontend (5/10~) ─→ QA (5/16~17)
                              ↗
              Backend (5/7~8) ─┘
```

이 문서는 **product spec**이다. 결정 reasoning, 거부된 alternatives, 운영 risk 등 의사결정 history는 [`PRD-v4.md`](./PRD-v4.md) (상세본) 참조. 5/8 디자인 논의 working doc은 별도 (`~/Desktop/prism-design-iteration/working-doc.md`).

**v3와의 주요 차이 (5/8 결정 반영):**
1. Frame shift — "흐름 매체 (시간상 변형)"에서 "프리즘 매체 (매체별 다각도)"로 무게중심 이동. 흐름은 그 안의 시간축으로 종속.
2. IA 3탭 신규 정의 — 홈 / 오늘의 이슈 / 피드. How We Work는 footer로 격하.
3. 흐름+반응 통합 — 별 탭이 아니라 흐름 step 안에 댓글 inline.
4. 매체 3시각 비교 신규 — AllSides 영감, 라벨 없이 매체명만 병치.
5. 정정 요청 두 종류 분기 — 기자에게 / PRISM에게.
6. 검색 부활 — 형태소 단위 (PRD lock), 데모는 가벼운 prototype.
7. 카드 콘텐츠 정의 명확화 — 단순 발표성 클레임 (양향자 type) 제외.

---

## 1. 한 줄 정의 ★ FRAME SHIFT

**PRISM은 같은 사실에 대해 다양한 매체가 어떻게 다른 각도로 다루는지를 매일 한국 정치 YouTube에서 보여주고, 그 분광과 시간상 흐름을 사용자가 직접 따라갈 수 있도록 펼치는 프리즘 매체다.**

- **프리즘 (분광)** = 같은 사실 (점) → 매체별 다른 시각 (다각도)
- **흐름 (시간)** = 그 분광이 시간상 어떻게 변형되어 퍼지는가
- 프리즘이 상위 메타포, 흐름이 그 안의 시간축

5/8 Jack 발화 [pt2 L2042-2045]: "필요한 건 나는 이게 흐름 매체여서 흐름을 보여주는 게 제일 중요하다는 생각을 했는데, 오늘 너랑 얘기를 해보니까 이 프리즘이 더 중요하긴 하네. 다양한 사실에 대해서 여러 가지 계획하고 펴잖아요."

---

## 2. Problem

한국 정치 YouTube에서 같은 사실이 매체별로 다른 수치·다른 framing으로 등장해 빠르게 변형되며 확산되지만:

1. **"같은 사실을 어떤 매체가 어떻게 다르게 보는가"** 를 보여주는 도구가 한국에 없다.
2. **"어디서 시작해 어떻게 변형되어 퍼졌나"** 를 시간축으로 보여주는 도구도 없다.

가장 가까운 시도들은 모두 다른 layer:
- 팩트체크 도구 (SNU팩트체크·JTBC팩트체크·뉴스타파): True/False 라벨링 + 사후 + 느림. 매체 다각도 비교 X.
- 뉴스 큐레이션 (뉴닉·슬로우뉴스·피렌체의 식탁): 사회 일반 + 흐름 안 보여줌 + 매체 비교 X.
- ChatGPT/Perplexity: Korean political graph 없음, hallucination, 매체 분광 X.
- AllSides (미국): 매체 좌우 라벨링 — PRISM은 이 라벨 자체를 거부.

**한국에 "같은 사실의 매체별 분광 + 그 변형 흐름"을 매일 다루는 매체가 없다.** PRISM이 채우는 white space.

2026년 6월 지방선거는 이 공백이 실제 투표 행동에 영향을 미치는 고위험 구간.

---

## 3. Target Users

### 1차: 정치 관심 minority (전체 유권자 5-15%)
- 이미 정치 YouTube 소비 + misinformation 신호에 민감
- "이 영상이 어디서 시작된 이야기인지", "다른 매체는 어떻게 다뤘는지" 확인하고 싶은 동기
- 카톡으로 정치 콘텐츠 공유 습관

### 2차 (이슈 framing으로 진입): 정치 관심 적은 20-30대
- 본인 삶 직결 이슈 (주거·노동·AI 규제 등)로 진입
- 이슈 라벨 시스템이 매개
- 흐름+반응 통합 layer가 felt pull로 작동

### 3차: 언론·연구자 (post-launch organic)

**처음부터 mass를 노리지 않는다.** Minority의 자발 사용 + 카톡 공유 → D-7~D-1 자연 mass 확장.

**현미 정리 7개 페르소나** (5/8 working doc 기반): 정치 minority 열성 / 카톡 공유자 / 이슈 진입자 / 신뢰 매체 갈증 / 큐레이션 신뢰 / 언론 종사자 / 연구자. 디자인 단계에서 페르소나별 entry path 검증 필요.

---

## 4. What We Make

### 4.0 Information Architecture — 메인 nav 3탭 ★ NEW

```
[홈]  [오늘의 이슈]  [피드]                    Footer: How We Work / 정정·제보
```

| 탭 | 무엇 | 출처 |
|---|---|---|
| **홈** | 오늘 추적 중인 5개 이슈를 한눈에. Hero 분광 시각화 + 카드 5개 요약 + N일째 카운터. 검색은 화면 하단 또는 sticky bar. | 새로 디자인 |
| **오늘의 이슈** | 매일 5개 카드를 옆으로 넘기는 deck 형식 (PC: 3D 카드 / 모바일: 가벼운 swipe). | Jack 데모 deck 패턴 살림 |
| **피드** | 주제별로 묶인 카테고리 피드 (8개 이슈 카테고리). 카드 teaser + 클릭 → 상세. | 현미 데모 feed.html 살림 |

**How We Work는 footer 링크**. 메인 nav에 있을 만큼 진입 우선순위 높지 않음 (이용약관 같은 류). 단 콘텐츠 자체는 유지 — 사람이 검수한다는 명시는 법적·명예훼손 mitigation으로 필수 (§4.3 Process Transparency Disclosure).

**카테고리 첫날 데이터 부족 — 의도된 sparse**: 피드를 카테고리별로 묶으면 첫 며칠은 한 카테고리에 1-2개일 수 있음. 카드 늘려서 보충하지 않음. 카테고리가 잘 묶인 것 자체가 의미 있는 데이터.

**로그인 / 유저 단위 관리 X**: "최대한 모든 정보는 준비되어 있고 그냥 와서 보면 되는" 형태. 단 알림 구독 (이메일 / 카톡 채널 / RSS)은 로그인 없이 가능.

---

### 4.1 평일 카드 (월~금 매일 5장)

모바일 우선, 카톡·X 공유 최적화. 구성 (필수 12 + 조건부 2 + 신규 3):

**필수 12 (v3 carry-over):**
1. **이슈 라벨** — 진입 식별자 (예: "주거")
2. **N일째 추적 카운터** — 카드 좌상단 ("📍 14일째 추적 중")
3. **카드 제목 — 수치 trajectory 포함** — 변형 폭이 제목에 직접 (예: "청년 무주택 통계: 35% → 50% → 60% (3일간)")
4. **카드 hero — 수치 trajectory 시각화** — 제목 아래 mini chart
5. **본문 (200-400자)** — factual 기술
6. **클레임 병렬 제시** — 2-4개 클레임 (발화자·발화일·발언)
7. **시간 순서 메타** — "가장 먼저 등장 / 24시간 후 / 익일 변형 / 주말 동안 확산"
8. **출처 성격 메타** — "정치인 직접 / 언론 인용 / 익명 커뮤니티 / 2차 가공"
9. **Canonical source 병렬** — 통계청·부처·후보 공식·data.go.kr 등 1-3개
10. **출처 링크 (전부)** — 영상 + 원본
11. **Provenance footer** — "PRISM은 발언을 분류하지 않습니다 / 출처를 보여드립니다 / 사람이 검수했습니다."
12. **공유 버튼** — 카톡, X, 링크 복사

**신규 3 (5/8 결정):**
13. **흐름+반응 통합** (§4.8) — 흐름 step 옆에 그 매체/영상의 댓글 1-2개 inline. 별 탭 X.
14. **매체 3시각 비교** (§4.7) — 같은 클레임을 다룬 매체 3곳의 시각 병치 (라벨 X, 매체명만).
15. **정정 요청 채널 분기** (§4.6) — 기자에게 / PRISM에게 두 갈래.

**조건부 (검토 후 채택):**
- (a) **영상 썸네일** — 각 클레임 옆에 YouTube 썸네일. Recognition moment. 저작권·캐싱 김재훈 검토. 5/8 현미 제안: 영상 썸네일 + 헤드라인 + 배댓 형식이 글자만 있는 것보다 한눈에 들어옴.
- (b) **분광 아이콘** — 카드 좌상단 작은 PRISM 분광 SVG. 디자인 quality bar 통과 시만.

**제외 (5/8 결정 — PRISM 영역 정의):**
- **단순 사실 발표성 클레임** (예: "양향자 국민의힘 경기지사 후보 확정")
  - 출처 1개 (선관위)에서 나온 명백한 팩트
  - 다양한 시각이 충돌하지 않음
  - 프리즘 메타포의 본 영역이 아님
  - Jack v3 데모의 양향자 카드는 다음 iteration에서 제외.

**카드 안 인터랙션 (5/8 결정):**
- 카드 자체가 옆 swipe (deck level) + 카드 내부도 가로 nested swipe = **불편하다는 비판 수용**.
- 카드 안에서는 흐름 / 확산 / 출처 / 반응 / 관계를 **세로로 stack** 또는 **위쪽 탭 클릭**으로 전환.
- 현미 데모 claim.html 패턴 채택. 가로 nested swipe 제거.

**톤** (v3 carry-over): 표준 존댓말 + factual + 인간미. 캐릭터 voice X, 위트 X, 풍자 X. 정치 평가 표현 ("우려된다", "심각하다") 금지.

**Voice direction (5/8 결정)**: "추적합니다" → "따라가요" 톤. 현미 voice가 Jack 의도와 더 가까움. Jack 본인이 v3 hero 카피 "오늘 다섯 가지 트렌드를 추적했습니다"를 자기 비판함 [pt1 L143-147]. v4에서 Jack 데모 voice 전체 개정.

---

### 4.2 토요일 Weekly Flow (주 1장)

그 주 가장 활발히 변형된 클레임 1개를 깊게 추적:
- 한 주 평일 카드 25-30장 중 가장 활발한 클레임 1개 선정
- 시간 축 timeline (월요일 등장 → 화요일 변형 → 수요일 확산 → 목요일 카운터 → 금요일 진정)
- 각 단계에 영상 link + canonical source 비교
- "이번 주의 흐름" 헤더로 흐름 매체 정체성 직접 surface
- 토요일 오전 9시 드롭 (주말 카톡 peak)

**시각 강화 (5/8 결정)**: v3 weekly card는 텍스트 위주 — 약함. v4에서 시각 unit 강화 (Saetbyeol 합류 후 final lock, 5/11+).

**Knowledge Graph는 위클리에 (5/8 결정)**: 개별 카드에서는 KG 빼고 (단일 주제 주변 노드만 보여주는 게 의미 약함). 위클리에서 여러 이슈 묶을 때 KG 활용. 이슈끼리 연결되는 인물·주제가 시각화되면 의미 있음.

**리더보드 (Hook #6 carry-over)**: "이번 주 가장 멀리 간 클레임 / 가장 빠르게 변형된 / 가장 많이 인용된 source" — 사실 metric only. 평가 ranking 금지.

평일 카드를 raw material로 재활용 → 신규 production 부담 최소화.

---

### 4.3 "How We Work" 페이지 (Process Transparency Disclosure)

인간이 어디에서 어떻게 검수·관리하는지 절차 자체를 공개:

**섹션 (Jack 데모 + 현미 데모 합집합):**
1. 우리는 무엇인가 (프리즘 매체)
2. **우리는 무엇이 아닌가 (5: 팩트체크 / 후보 평가 / 투표 추천 / 분류 / 광고 매체)** — 광고 매체 아님 명시 (현미 추가, v4 채택)
3. 매일의 작업 흐름 (08:00~12:00 daily workflow)
4. 검수 체크리스트 (approver checklist 6개) — 발행 전 통과 + 다른 팀원 1명 approval
5. 우리가 신뢰하는 출처 (canonical 7종 + data.go.kr)
6. 우리가 사용하는 AI (자동화 항목 + 자동화 안 하는 항목)
7. **팀 (6명: Jack / 재훈 / 정아인 / 김현미 / Saetbyeol / 샘)** — 현미 추가, v4 채택
8. PRD 변경 history (GitHub public repo)
9. 정정·제보 channel (두 종류)
10. 아직 구현되지 않은 영역 (자기 제한 명시)
11. 책임자 강동재 + hello@prism.kr

**노출 방식 (5/8 변경)**: **메인 nav에서 빼고 footer 링크로**. 진입 modal 강제 X. 메인 nav 진입 우선순위는 콘텐츠 (홈/오늘의 이슈/피드)에 양보.

**Dual purpose** (carry-over): (1) 객관성 추구 의도를 절차 가시화로 implicit signaling, (2) 사전 자기 보호 (legal alibi). 사후 변명이 아니라 사전 표명.

---

### 4.4 "Show, don't classify" 원칙 (carry-over)

PRISM은 어떠한 평가 라벨도 부여하지 않는다.

- ❌ "왜곡됨", "거짓", "오해 소지", "True/False/Misleading", "사실과 다름", "검증됨"
- ✅ "다음과 같은 통계가 있습니다", "통계청 자료에 따르면", "오마이뉴스 4월 28일 기사에 따르면 발언이 있었다고 합니다" (*보도 사실* 인용)

**왜:**
1. 법적 risk 최소화 (명예훼손법 + 공직선거법 250조·82조의4는 모두 *판단·평가 진술*에 적용)
2. 사용자 자율성 존중 — 가르치는 톤 0
3. 운영 부담 감소 — daily intensive cadence 가능

**5/8 강화 — 매체 3시각 라벨링 X (§4.7 참조)**: AllSides 같은 외부 좌·우 라벨도 PRISM은 차용하지 않는다. 매체명만 병치 + 시각만 비교.

---

### 4.5 Hook & Felt Pull layer (8개)

정치 관심 적은 2030 secondary target의 felt pull을 의도적으로 높이는 layer. **재미가 아니라 substance의 surprise + 자기 정체성 신호**. 프리즘 매체 정체성 (§1)과 같은 axis 위에 있는 hook만 채택.

**8 hook (frame shift 반영):**

1. **카드 hero 수치 trajectory** — 제목 + mini chart에 변형 폭 직접 노출 ("35% → 50% → 60%"). Scroll에서 즉시 인지. (carry-over)

2. **N일째 추적 카운터 (3-layer 통합)**
   - Site-level: 사이트 홈 헤더 ("추적 중인 클레임 24개, 가장 오래된 것 21일째")
   - Card-level: 카드 좌상단 라벨 ("📍 14일째 추적 중")
   - Weekly Flow-level: 토요일 카드 핵심 지표 ("이 클레임 N일째 N차 변형")
   - Backend schema에 클레임 ID + 시작일 + 변형 history 박힘 필수.
   (carry-over)

3. **사이트 hero — "오늘의 분광" 시각화 ★ frame shift 반영**
   - 진입 즉시 보이는 첫 visual element. 카드 list가 아니라 **분광 자체가 첫 인상**.
   - 중심 = canonical source (사실), 바깥 = 매체별 변형 클레임 (다각도). 프리즘 메타포와 시각적 일치.
   - v3 SpectrumHero (추상 5색 광선 일러스트 + "한 줄기 빛이 다섯 색으로 갈라집니다") = **5/8 폐기**. 추상 비유가 Jack 본인이 가장 비판한 anti-pattern [pt1 L143-147].
   - v4 hero 후보:
     - (A) 현미 데모 spread SVG 강화 — 오늘 실제 클레임의 4갈래 분광 (통계청 35% 중심 → 국토부 38% / 후보X 50% / 채널Z 60%)
     - (B) Sankey flow + 분광 hybrid
     - 데모 단계에서 prototype 비교 후 quality 우월 안 final.

4. **PRISM 분광 메타포 — Design Language (조건부 채택)**
   - Logo + 사이트 hero + 카드 hero corner + brand voice ("이번 주의 분광")
   - **Quality bar 통과 시만 채택.** 미달 시 drop, 단순 typography brand identity로 fallback.
   - 색상 가이드: 무지개 X (진영색 회피, 5/6 lock). 후보 — monochrome / 단일 cool tone (청록·보라·남색) / 출처 성격별 mapping.

5. **사이트 hero 한 줄 lede (매일 갱신, factual only) ★ voice 변경**
   - 신문 1면 lede 효과. 큰 글자 한 줄.
   - v3 폐기: "오늘 다섯 가지 트렌드를 추적했습니다." (Jack 본인 비판)
   - v4 채택 패턴 (현미 voice): "오늘은 청년 무주택 통계가 네 갈래로 흘러갔어요." — 그날의 구체 스토리를 lede로.
   - 평가 표현 금지.

6. **Weekly Flow 리더보드 (사실 metric only)** (carry-over)
   - "이번 주 가장 멀리 간 클레임 / 가장 빠르게 변형된 / 가장 많이 인용된 source"
   - 평가 ranking 금지 ("가장 왜곡된" 등 X).

7. **카톡 미리보기 강화 (Open Graph image)** ★ 5/8 명시
   - 큐레이션 워크플로우에 "공유 한 줄" 항목 추가. 본문과 별개로 카톡 description 슬롯.
   - **og:image 자동 생성 템플릿**: 카드 trajectory 시각화 + 헤드라인. 단순 텍스트가 아니라 **카드 미리보기가 자동으로 예쁘게 나오는 형태**.
   - 미스인포메이션 해결 = 좋은 정보가 쉽게 퍼지는 것. 카톡은 한국인 1순위 분배 채널.
   - 백엔드 자동 생성 가능성 김재훈 확인 필요.
   - factual only. 평가 표현 금지.

8. **정정 요청 채널** (§4.6으로 분리, 신뢰 loop)

**거절된 hook:**
- 사용자 제보 → 운영 부담 + identity 미스매치 (PRISM은 추적 매체)
- 카톡 채널 weekly push → Track 1.5 scope 외, post-launch deferred
- 무지개 색상 — 진영색 회피 lock (5/6, 5/8 둘 다 거부)

**Pre-launch backfill 필수 (5/24~5/29):** 시각화 + 카운터가 launch day 빈약하지 않으려면 시드 클레임 20-30개 backfill 작업 필요. Daily cadence 5장 → 4장 일시 축소 허용.

---

### 4.6 정정 요청 채널 ★ 두 종류로 분기 (5/8 신규)

v3는 단일 정정 요청. v4에서 두 갈래로 분기:

**A. 기자에게 정정 요청 (외부 매체 / 기자)**
- "이 부분 더 취재해주세요"
- 외부 매체·기자에게 새로운 자료·관점 요청 전달
- Misinformation actionable feature
- PRISM은 중개자 역할 (요청 내용을 요약·전달, PRISM 자체 검수 X)

**B. PRISM에게 정정 요청 (자체 카드 수정)**
- "이 카드 다시 업데이트해주세요"
- PRISM 큐레이터에게 카드 수정 요청
- Form 항목: 카드 URL + 정정 부분 + 근거 출처 link + (선택) 연락처
- 24h 내 큐레이터 검토 + 응답
- 검증된 정정은 retraction protocol에 따라 카드 revision (revision history visible)

**UI 분기**: 정정 요청 버튼 클릭 → 1차 선택 modal ("기자에게 / PRISM에게") → 분기된 form. 또는 두 버튼 분리.

신뢰 loop: "사람이 검수했다 + 사용자도 정정 가능 (두 채널)". Process Transparency Disclosure (§4.3)와 같은 axis.

---

### 4.7 매체 3시각 비교 ★ NEW (5/8 신규)

**무엇**: 같은 클레임을 다룬 매체 3곳의 시각을 같이 보여주기.

**예시**: "이준석 하버드 본록장에 대해 / 매체 A는 이렇게 / 매체 B는 이렇게 / 매체 C는 이렇게 다뤘다."

**원칙 (Show, don't classify §4.4와 정합):**
- **라벨 X** — AllSides 같은 외부 좌·우 라벨 차용 X. PRISM 자체 라벨링 X (원칙 충돌).
- **매체명 + 시각 병치만** — 라벨 없이 사용자가 자력 추론.
- 5/8 결정 옵션 3 (라벨 없이 매체명만 병치 + 시각만 비교) 채택.

**디자인 영역 (현미)**: 카드 안 어딘가에 자연스럽게 들어갈 layout. 평일 카드 §4.1 신규 항목 14에 포함.

**백엔드 영역 (재훈)**: 같은 클레임을 다룬 매체 3개 매칭 로직. data.go.kr + 7개사 정치 카테고리 RSS + 정치 YouTube 시드 채널 cross-reference. 자동 추천 + 큐레이터 final 선정. 백엔드 가능성 재훈 확인 필요 → 가능 시 Track 1.5 포함, 불가 시 post-MVP.

**매체 다양성** (5/8 결정): 언론사 + 유튜버 + 정치인 채널 모두 포함. 어떤 비율로 큐레이션할지는 큐레이터 영역.
- 5/8 Jack 입장 [pt2 L1671-1689]: "공신력 있는 매체이기 때문에 미스인포가 안 다룬다는 생각은 절대 안 해. MBC 뉴스도 안철수 의원 발언 보도하잖아. 빼는 게 오히려 더 인위적인 개입."

---

### 4.8 흐름+반응 통합 ★ NEW (5/8 신규)

**무엇**: v3는 FLOW (흐름) / SPREAD (확산) / TRACE (출처) / REACT (반응) / GRAPH (관계) **5탭 분리** 구조. v4에서는 **REACT를 별 탭으로 빼지 않고 FLOW step 안에 inline 통합**.

**근거** (5/8):
> 현미 [pt2 L1969-1973] 제안: "이 흐름에다가 반응을 같이 넣는 건 어때요? KBS 뉴스 7이라고 하면은?"
> Jack 즉답: "오 좋은데? 너무 좋아×2"

**예시 layout:**
```
03. 04.27 — KBS 뉴스 7
   "청년 무주택 35% 발표"
   ─────────────────────
   "통계청 자료랑 후보 발언 수치가 다른 이유가 뭐임?"
   — KBS 뉴스 7 영상 댓글 ♥ 88
```

흐름 step 하나마다 그 매체·영상의 댓글 1-2개를 바로 같이 보여줌. 별도 REACT 탭 안 들어가고 흐름 안에서 자연스럽게 읽힘.

**흐름 step 최대 7개 (5/8 결정)**: 이재명 대통령 당선 같은 이슈는 60개 채널이 다룰 수 있음. 60개 다 보여줄 수 없음. **최대 7개로 제한**. 7개는 변곡점·전환점 위주로 큐레이션. 큐레이션 정책이라 디자인 영역 X — 단 layout이 7개를 자연스럽게 다룰 수 있는 형태여야 함.

**시각 unit (5/8 현미 제안 채택)**:
- 영상 썸네일 (YouTube)
- 영상 헤드라인
- 배댓 (좋아요 많은 댓글) 1-2개

글자만 있는 layout보다 한눈에 들어옴.

**미해결 — 댓글 큐레이션 정책 (Open Question §14)**: 어떤 댓글을 어떻게 뽑을지, 프레이밍 효과 어떻게 피할지. 큐레이터 manual selection vs LLM-assisted top comment extract. 5/11 이후 calibrate.

**카드 5탭 → 4탭 변경**:
- v3: FLOW / SPREAD / TRACE / REACT / GRAPH
- v4: FLOW (반응 통합) / SPREAD / TRACE / GRAPH(weekly only)
- 카드 안에서는 4개를 세로 stack 또는 위쪽 탭 클릭. 가로 nested swipe X.

---

### 4.9 검색 ★ 부활 (5/8 PRD 결정)

**v3 결정**: MVP에서 검색 제외 (형태소 분석 비용·세팅 시간 + 6/3 선거까지 카드 100개라 카테고리+날짜로 탐색 가능).

**v4 변경**: **PRD 최종 product에는 형태소 단위 검색 포함**. 데모 단계에서는 가벼운 client-side prototype.

**근거 (5/8 Jack)**:
- 한국어 정치 콘텐츠는 매체별·정치인별 명사형이 다양 (예: "청년 주거 / 청년 무주택 / 청년 주택 부족"). 단순 string match로는 부족.
- 형태소 단위가 한국어 자연어 검색 표준. 1년+ 운영 시 검색 필수가 될 가능성 매우 높음.
- 운영 가치 / 구현 비용 trade-off 재평가 → 형태소 검색은 final product에 포함.

**구현 분기:**
- **MVP / Backend (재훈)**: 형태소 분석기 (mecab-ko / KOMORAN / khaiii 중 선택, 재훈 위임). 인덱스 빌드 워크플로우.
- **데모 prototype**: 가벼운 client-side filter (제목 + 본문 substring match + 이슈 라벨 filter). 형태소 분석은 배포 production 단계에서.
- **데모 검색 UI**: 홈 hero 하단 또는 sticky bar. 자동완성·suggestion은 post-MVP.

**탐색 보조 (검색 외)**:
- 8개 이슈 카테고리 chip
- 시간 필터 (오늘 / 이번 주 / 전체)
- N일째 sort

---

## 5. Sources & Method (carry-over)

### 5.1 Canonical Source 7종

PRISM이 신뢰하는 1차 출처. 모든 카드는 이 7종 중 하나 이상을 인용:

1. 정치인 공식 채널 (성명·보도자료)
2. 정당 공식 계정·보도자료
3. 주요 언론 직접인용 기사 (조선·중앙·동아·한겨레·경향 + 연합·뉴시스 wire)
4. 정치인·정당 공식 SNS (verified 계정)
5. 네이버 블로그 (정치인 본인 운영)
6. 국회 속기록 (접근 가능 범위)
7. 정부·대통령실·부처 공식 발표 (정책브리핑 등)

**Out of canonical:** TV 토론회 영상, 외신, Substack·개인 블로그, 익명 게시판, 검증되지 않은 커뮤니티 캡처.

### 5.2 Canonical Source Graph (사전 매핑)

PRISM의 가장 중요한 차별화 자산.

- **Nodes**: 후보자 / 정당 / 정부 부처 / 국회 / 주요 언론 / 공식 SNS 계정 / 정치인 블로그 / 공식 정책 발표 채널
- **Edges**: 누가 누구의 account를 운영, 누가 어느 정당, 어느 부처가 어느 정책 발표, 어느 언론이 어느 후보 어떻게 보도
- **Pre-loaded**: 선관위 2026 지방선거 후보자 전수 + SNS 매핑 + 정책브리핑 RSS + 정치인 블로그 RSS + 7개사 정치 카테고리 RSS

### 5.3 data.go.kr (공공데이터포털)

무료 키 활용:
- 국회 속기록·회의록 (정치인 발언 1차 출처)
- 정책브리핑 (정부 정책 archive)
- 선거 정보지 (선관위 공식 데이터)

**API 키 신청 진행 (5/10 PRD 확정 전).** BIGKinds API 사용 안 함.

### 5.4 정치 YouTube monitoring

- 시드 채널 40-60개 (좌·우 균형)
- YouTube 트렌딩 정치 카테고리 일일 review
- 후보자명 쿼리로 누락 보강
- 22개 신호 수집 (API + 파싱 + AI 파생)
- 크롤링 안 함 (법적 risk + 안정성)

### 5.5 매체 3시각 cross-reference 신규 (5/8)

§4.7 매체 3시각 비교를 위한 매칭 layer:
- 같은 클레임 = 같은 수치·인물·날짜 anchor
- 매체 = 7개 언론사 정치 카테고리 RSS + 정치 YouTube 시드 채널 + 정치인 공식 채널
- 자동 추천 (LLM-assisted) → 큐레이터 final 선정

---

## 6. Operations

### 6.1 일일 워크플로우

```
오전 8:00  큐레이터 lead (Jack) data.go.kr + YouTube monitoring 결과 review
오전 8:30  AI assist로 trending claims 자동 추출 → 큐레이터 검토 큐
오전 9:00  큐레이터 lead가 그날 카드 후보 10-15개 선정
오전 9:30  AI assist로 카드 초안 자동 작성
              + 흐름 7 step 큐레이션 (변곡점 위주)
              + 매체 3시각 후보 cross-reference
              + 카톡 OG image hook 한 줄
오전 10:00 큐레이터 lead가 5장 final 선정 + 인간미·톤 가다듬기
오전 11:00 다른 팀원 1명이 publish gate review
정오 12:00 5장 publish + 카톡 공유 trigger
```

**토요일** (Weekly Flow):
```
오전 7:00  Lead가 한 주 카드 25-30장 review
오전 8:00  가장 활발히 변형된 클레임 1개 선정
오전 8:30  Weekly Flow 카드 작성 (시간축 + 변형 timeline + KG 시각화)
오전 9:00  Approver review + publish
```

### 6.2 1 human approver gate (carry-over)

> PRISM card는 최소 1명의 human approver의 명시적 approval 없이 publish 되지 않는다.

**Approver checklist (8개, v4에서 2개 추가):**
1. 클레임 원본 영상 link 작동 + 발언 정확성
2. Canonical source 원본 link 작동 + 인용 정확성 (data.go.kr 포함)
3. 분류 라벨 포함 여부 (§4.4 위반 감지)
4. 정치 평가 표현 포함 여부 (톤 가이드라인 위반 감지)
5. 후보 평가·투표 추천 함의 포함 여부
6. 명예훼손 위험 평가
7. **흐름 7 step이 변곡점 위주인지** ★ NEW (단순 시간순 60개 X)
8. **매체 3시각 라벨 위반 여부** ★ NEW (좌·우 라벨 등 평가 라벨 0)

**Self-approval 금지.** Approver 이름은 카드 metadata에 기록.

### 6.3 흐름 7 step 큐레이션 정책 ★ NEW

흐름 step 최대 7개 (§4.8). 큐레이터 manual selection이 기본:
- **변곡점 위주**: 시간순 모든 영상이 아니라 시각·수치·매체 종류가 바뀌는 지점
- **자동 신호 후보** (재훈 backend): 시간 delta + 출처 grade 변화 + view 급증 + 댓글 sentiment shift
- **큐레이터 final**: 자동 신호 + manual selection 합쳐서 7개 lock

**MVP**: 큐레이터 manual selection 100%. Post-MVP에 자동 신호 도입.

### 6.4 AI 활용 (carry-over + v4 추가)

**자동화 (AI):**
- data.go.kr / Canonical Source Graph 자동 lookup
- YouTube trending claims 자동 추출
- 카드 초안 자동 작성 (claim + source 병렬)
- 시간 순서 / 출처 성격 메타 자동 분류 (큐레이터 검수)
- 톤·스타일 일관성 check
- 분류 라벨 위반 자동 감지
- 카톡 미리보기 메타데이터 + og:image 자동 생성 ★ NEW
- **매체 3시각 cross-reference 후보 자동 추천** ★ NEW
- **흐름 step 자동 신호 (변곡점 후보) 추천** ★ NEW (post-MVP)

**자동화 안 함 (사람):**
- 그날 publish할 5장 (+ 토요일 1장) final 선정
- 카드 톤·인간미 최종 가다듬기
- 시간 순서 / 출처 성격 메타 최종 검수
- **흐름 7 step 변곡점 final 선정** ★ NEW
- **매체 3시각 final 선정** ★ NEW
- **댓글 1-2개 final pick (흐름+반응 통합용)** ★ NEW
- Publish gate approval
- Risk 판정 (명예훼손·정치 평가)

---

## 7. What We Are Not (5가지, carry-over)

PRISM의 정체성 lock. 외부 압력에 대한 1차 응답 자료.

1. **사회·경제 zeitgeist 도구가 아니다.** 정치와 무관한 사회·경제 이슈 자체는 다루지 않는다.
2. **후보 평가 도구가 아니다.** "이 후보가 좋다 / 나쁘다" 판단을 제공하지 않는다.
3. **팩트체크 도구가 아니다.** True/False 라벨링하지 않는다. 명시는 사이트 진입 시 사용자 facing 노출.
4. **투표 추천 도구가 아니다.** 투표소 알림 등 선거기 한정 기능도 추가하지 않는다.
5. **분류 도구가 아니다.** Show, don't classify.

**+ "광고 매체가 아니다" (5/8 현미 제안 채택)**: How We Work 페이지에 명시. PRISM 콘텐츠는 광고·sponsored content를 받지 않음.

---

## 8. Team

| 역할 | 담당 |
|---|---|
| Lead + 큐레이션 큐레이터 | Jack Kang (강동재) |
| Backend / pipeline 리드 | 김재훈 (개발 리딩 + 합류 멤버 협업 방식 결정 권한) |
| Governance + 이슈 큐레이션 보조 | 정아인 |
| 디자인 / UX 리드 | 김현미 |
| Provenance UI / 시각 디자인 리드 | Saetbyeol Leeyouk (MIT Media Lab) — **5/11 본격 합류** |
| Frontend / 개발 | 샘 (Sam Lee, EPFL Computer Science) — 이번 주부터 함께 작업 |

6명 모두 학업·직장·다른 프로젝트 병행. 일정은 마일스톤 단위.

**Jack daily commitment**: launch 기간 (5/4 ~ 6/3) 매일 lead 큐레이션 직접 담당. 학업 충돌 시 사전 공지 + 백업 큐레이터 (정아인) 지정.

**정기 미팅 슬롯**: KST 평일 21:00–24:00 (= NYC 오전 8:00–11:00).

**5/11 Saetbyeol 합류 후 visual lead 분담** (5/8 명시): 김현미 = UX·IA·콘텐츠 구조. Saetbyeol = 시각 디자인 (typography·color·shadow·design system). Jack = 큐레이션·콘텐츠·voice. 재훈 = backend·frontend stack 결정. 샘 = frontend 개발.

---

## 9. Milestones

| 시점 | 마일스톤 |
|---|---|
| **2026-05-06 (화)** | PRD v3 lock |
| **2026-05-07 (수) ~ 5/8 (목)** | Backend pipeline 시작 (재훈 리드, parallel) |
| **2026-05-08 (목)** | **★ 5/8 디자인 논의 완료 + PRD v4 lock** + Hi-Fi 시작 |
| **2026-05-09 (금) ~ 5/10 (토)** | **🔴 Demo hard-deadline.** 디자인 + hook 통합 작동 데모 (병합 v3) |
| **2026-05-10 (토)** | Frontend 통합 + 김현미 디자인 디테일 1차 점검 |
| **2026-05-11 (월)** | Saetbyeol 합류. 6명 team kick-off + 데모 walkthrough + visual lead 인계 |
| **2026-05-16 (토) ~ 5/17 (일)** | **🔴 QA + final polish.** Beta invite 직전 상태. |
| **2026-05-18 ~ 5/23** | Beta 5-10명 invite + feedback 반영 + bug fix |
| **2026-05-24 ~ 5/29** | Pre-launch backfill (시드 클레임 20-30개) + 한국 변호사 check |
| **2026-05-30 (토)** | **Public launch.** Daily 5장 cadence 시작 |
| **2026-06-01 ~ 6/2** | Election week escalation (daily 7-10장) |
| **2026-06-03 (수)** | **선거일.** 실시간 monitoring + 사후 정리 |
| **2026-06-04 ~ 6/10** | Mandatory rest week + retro |

**이번 주 협업 흐름 (5/8~5/11):**
1. **Jack** — PRD v4 lock + Claude Code 데모 v4 빌드 (병합 v3)
2. **김현미** — PRD v4 + 데모 v4 검토 → 디자인 다음 iteration (5/9 저녁부터)
3. **재훈** — Backend skeleton + 매체 3시각 / 카톡 OG / 흐름 7 step 큐레이션 가능성 회신
4. **샘** — frontend stack 결정 + 합류 prep
5. **5/11** — Saetbyeol 합류 + 6명 kick-off + 데모 walkthrough + 시각 디자인 lead 인계

---

## 10. Launch Readiness Checklist

Public launch (5/30) 전 필수 통과:

- [ ] PRD v4 확정 (5/10 전 final lock)
- [ ] Daily 5장 cadence 1주 연속 안정 운영
- [ ] Weekly Flow 토요일 1장 첫 발행 성공 (5/24 또는 5/31)
- [ ] Approver gate 모든 카드 적용 + checklist 8개 통과율 100%
- [ ] 분류 라벨 위반 0건 (지난 1주)
- [ ] 카톡 미리보기 정상 작동 + og:image 자동 생성 ★ v4
- [ ] Canonical Source Graph 후보자 매핑 ≥80% 완성
- [ ] data.go.kr API 키 발급 + 자동 lookup 작동
- [ ] About / How We Work 페이지 footer 노출 (메인 nav X)
- [ ] 한국 변호사 preliminary check 완료
- [ ] 8개 이슈 카테고리 MECE 재검증 완료
- [ ] 6명 팀 burn-out 신호 monitoring (weekly retro)
- [ ] 사이트 hero "오늘의 분광" 시각화 작동 + 시드 클레임 backfill 완료
- [ ] N일째 카운터 site/card/Weekly Flow 3-layer 모두 작동
- [ ] 분광 메타포 디자인 quality bar 통과 또는 drop 결정 (5/11+ Saetbyeol)
- [ ] 카드 hero 수치 trajectory 시각화 작동
- [ ] 사이트 hero 한 줄 lede 매일 갱신 워크플로우 정착 (현미 voice)
- [ ] Weekly Flow 리더보드 form factor 작동
- [ ] **흐름+반응 통합 layout 작동** ★ v4
- [ ] **매체 3시각 비교 작동** (라벨 X) ★ v4
- [ ] **정정 요청 두 종류 form 작동** ★ v4
- [ ] **검색 형태소 단위 인덱스 작동** ★ v4
- [ ] (조건부) 영상 썸네일 저작권/캐싱 검토 완료 + 채택/거절 결정

---

## 11. Differentiation (한 줄)

> **PRISM은 같은 사실에 대해 매체별로 다른 시각이 어떻게 펼쳐지는지를 매일 보여주는 프리즘 매체다.** 매체 다각도 (분광) + 시간상 변형 (흐름) 을 분류 없이, 매체 라벨 없이, 시간 순서 + 출처 성격 메타와 함께, canonical source graph + data.go.kr 기반으로 제공한다. 프리즘 매체 정체성을 매일 visible한 hook (분광 시각화 hero + N일째 카운터 + 매체 3시각 + 흐름+반응 통합) 으로 surface. 한국에 동일 layer의 매체 부재 — white space.

| 도구 | 강점 | PRISM 대비 약점 |
|---|---|---|
| ChatGPT / Perplexity | Universal Q&A, 빠름 | Korean political graph 없음, hallucination, 매체 분광 X, 흐름 X |
| 뉴닉 | Daily news, 가벼운 톤 | 사회 일반, 정치 미스인포 specific X, 매체 비교 X |
| 슬로우뉴스 | 깊은 시사 분석 | Weekly+ cadence, 매체 다각도 비교 X |
| SNU팩트체크 / JTBC팩트체크 / 뉴스타파 | 깊은 fact-check | True/False 라벨, 사후, YouTube 분석 X, 매체 비교 X |
| AllSides (미국) | 매체 분광 시각 | 좌·우 라벨 차용 (PRISM은 라벨 거부), 한국 정치 없음 |

**우리는 같은 사실에 대해 매체가 어떻게 다른 각도로 펼치는가 (how it spreads + how it transforms) — 한국에 prior art 없음.** 프리즘 매체 정체성을 시각화·매체 3시각·흐름+반응 통합으로 사용자가 매일 *느낄 수 있게* 한다.

---

## 12. Tech Stack (high-level)

```
[YouTube Data API v3 + yt-dlp 백업]
        ↓
[Discovery Cascade: 시드 채널 + 트렌딩 + 후보자명]
        ↓
[22-signal Collection + AI 파생]
        ↓
[Canonical Source Graph + data.go.kr cross-reference + 매체 3시각 매칭]
        ↓
[Claim Matching + 시간 순서 / 출처 성격 메타 자동 분류]
        ↓
[흐름 step 변곡점 자동 신호 (post-MVP)]
        ↓
[형태소 검색 인덱스 (mecab-ko / KOMORAN / khaiii — 재훈 위임)]
        ↓
[AI assist 카드 초안 → 큐레이터 final → Approver gate (8개 checklist)]
        ↓
[og:image 자동 생성 (카드 trajectory + 헤드라인)]
        ↓
[Public Web (반응형, 카톡 공유 최적화)]
```

**Web only** (앱 X). 로그인·personalization 없음.

세부 기술 선택 (KG DB / embedding 모델 / LLM / frontend stack / 형태소 분석기) 은 개발 리드 (재훈) 위임.

---

## 13. Constraints & Risks (요약, carry-over)

**Legal**: 공직선거법 82조의4 + 250조 + 한국 명예훼손법 — 모두 *판단·평가 진술*에 적용. Show-don't-classify (§4.4) + 매체 3시각 라벨 X (§4.7) 가 가장 큰 mitigation. 한국 변호사 preliminary check 필수.

**Operational**: 6명 모두 part-time. Daily cadence + 토요일 Weekly Flow의 burn-out risk. Approver rotation + 일요일 휴식 + Election week 후 mandatory rest week로 mitigation. **v4 신규 부담**: 흐름 7 step manual 큐레이션 + 매체 3시각 cross-reference + 댓글 final pick. 자동 신호 도입 (post-MVP)으로 부담 단계적 절감.

**Product**: 8개 이슈 카테고리 MECE 부족 가능성 (5/10 전 재검증). Process Transparency Disclosure가 "AI 자동화 못 한다" 신호로 역효과 가능성 (페이지 framing으로 mitigation). **v4 신규 risk**: 매체 3시각 매칭 자동화 한계 — 같은 클레임을 다른 framing으로 다룬 매체 식별이 LLM에 어려움. MVP는 큐레이터 manual fallback.

자세한 risk + mitigation은 [`PRD-v4.md`](./PRD-v4.md) Appendix C 참조.

---

## 14. Open Questions (5/8 미해결 8개)

| # | Open Question | Decision Owner | Target Lock 시점 |
|---|---|---|---|
| 1 | 댓글 큐레이션 정책 (어떤 댓글 어떻게 뽑을지, 프레이밍 효과 어떻게 피할지) | Jack + 김현미 + 큐레이터 팀 | 5/16 QA 전 |
| 2 | 5개 카드를 한눈에 vs 디테일 한번에 (홈 layout) | Jack + 김현미 (디자인 try) | 5/11 데모 walkthrough |
| 3 | Weekly Flow 카드 시각화 방식 | 김현미 + Saetbyeol | 5/16 QA 전 |
| 4 | 임팩트 있는 hero (프리즘 메타포 visualize) — A안 vs B안 | Jack + 김현미 + Saetbyeol | 5/11 Saetbyeol 합류 후 |
| 5 | Layered depth (점진적 disclosure) — MVP 보류, post-MVP 검토 | 전체 | post-MVP |
| 6 | 매체 3시각의 라벨링 (라벨 X 결정 lock) — 매체명 표기 외 추가 메타 어디까지? | Jack | 5/16 QA 전 |
| 7 | 카드 헤드라인 자동 추출 vs 큐레이터 작성 | 재훈 + 큐레이터 팀 | 5/11 데모 후 |
| 8 | 인스타 공유 / 팔로우 등 추가 액션 (이모지 사용 정합성) | 김현미 | 5/11 디자인 final |

**재훈 의존 (5/8 Jack 약속)**: 매체 3시각 백엔드 가능성 / 카톡 OG image 자동 생성 / 흐름 7 step 큐레이션 자동 신호 — 재훈 회신 후 v5 또는 patch.

---

## 15. Design Decisions Log (5/8 13개 lock)

5/8 Jack ↔ 김현미 디자인 논의 (2시간 35분, ~2,635 transcript segments) 결과. Working doc: `~/Desktop/prism-design-iteration/working-doc.md`.

| # | 결정 | Transcript 라인 | v4 섹션 |
|---|---|---|---|
| 1 | 메인 nav 3탭 (홈 / 오늘의 이슈 / 피드). 소개 → footer | [pt2 L501-549] | §4.0 |
| 2 | 검색 — MVP 부활 (형태소 단위, PRD lock) | (5/8 추가 결정) | §4.9 |
| 3 | 로그인 / 유저 단위 관리 X | [pt2 L864-874] | §4.0 |
| 4 | 카드 5장 deck 형식 유지 + 모바일 가벼운 swipe | [pt1 L7-12, L446-457] | §4.1 |
| 5 | 카드 안 nested 가로 캐러셀 → vertical stack + 위쪽 탭 | [pt1 L121-131, pt1 L156-162] | §4.1 |
| 6 | 흐름+반응 통합 (별 탭 X, 흐름 step inline 댓글) | [pt2 L1969-1973] | §4.8 |
| 7 | 흐름 step 최대 7개, 변곡점 위주 | [pt2 L1975-1985] | §4.8, §6.3 |
| 8 | 영상 썸네일 + 헤드라인 + 배댓 시각 unit | [pt2 L1949-1957] | §4.1, §4.8 |
| 9 | 매체 3시각 비교 (AllSides 영감, 라벨 X) | [pt2 L1949-1957, pt2 L2024-2030] | §4.7 |
| 10 | 카드 콘텐츠 정의 (단순 발표성 클레임 제외, 양향자 type X) | [pt2 L1011-1030] | §4.1 |
| 11 | 매체 다양성 (언론사 + 유튜버 + 정치인 채널 모두 포함) | [pt2 L1671-1689] | §4.7 |
| 12 | 카톡 OG 미리보기 자동 생성 강화 | [pt2 L1907-1917 인접] | §4.5 hook #7 |
| 13 | 정정 요청 두 종류 분기 (기자 / PRISM) | [pt1 L298-326] | §4.6 |
| 14 | Weekly Flow 시각 강화 (Saetbyeol 후 final), KG는 위클리 | [pt1 L329-359, pt2 L1907-1917] | §4.2 |
| 15 | Frame shift "흐름 매체 → 프리즘 매체" | [pt2 L2042-2045] | §1, §11 |
| 16 | Voice "추적합니다 → 따라가요" (현미 voice가 Jack 의도와 가까움) | [pt1 L143-147, pt2 throughout] | §4.1, §4.5 |

**Anti-patterns lock (5/8):**
- 바이브 코딩 느낌 (Claude로 빨리 뽑은 사이트 톤)
- 챗GPT 스타일 / 라이브 위키
- 불꽃 이모티콘 / 일반 이모티콘 헤드라인
- 글자만 가득한 카드 (정보 한눈에 X)
- 모든 네모가 똑같이 생긴 카드
- "한 줄기 빛이 다섯 색으로 갈라집니다" 같은 추상 비유 멘트
- "오늘 다섯 가지 트렌드를 추적했습니다" 같은 형식적 헤드라인
- True/False 라벨링 / 분류 톤
- 카드 가로 nested swipe

**Preferences lock (5/8):**
- Apple Music 앨범아트 스타일 3D 카드 넘기기 (PC만)
- 친근한 voice ("따라가요")
- 시각 요소 풍부 (썸네일, 이미지, 차트, 분광)
- 한눈에 정보가 들어오는 layout
- "Show, don't classify" 일관

---

## Document Metadata

**Version**: v4.0 Core (Frame Shift "프리즘 매체" + IA 3탭 + 흐름+반응 통합 + 매체 3시각 + 검색 부활)
**Last updated**: 2026-05-08 (NYC, 5/8 Jack ↔ 김현미 디자인 논의 통합)
**Authors**: Jack Kang + Claude (writing assist, all decisions Jack-led, 5/8 김현미 협업)
**Source decisions**:
- 2026-05-05 [[김재훈]] 콜 (data.go.kr / 팩트체크 X 명시 / web 빌드)
- 2026-05-06 hook layer brainstorm + timeline lock + 5명 합집합 협업 흐름
- 2026-05-08 [[김현미]] 디자인 iteration (2h 35m, 13 decisions + 8 open questions + frame shift)
**Demo hard-deadline**: 2026-05-09 ~ 05-10 (다음 작업 — 같은 세션에서 데모 v4 빌드)
**Wrap-up**: 2026-05-16 ~ 05-17 (QA + final polish)
**Public launch**: 2026-05-30 (선거 D-4)
**Companion documents**:
- [`PRD-v4.md`](./PRD-v4.md) — 결정 reasoning, 거부된 alternatives, risk 상세
- `~/Desktop/prism-design-iteration/working-doc.md` — 5/8 디자인 논의 working scaffold (Claude 협업용)
- `~/Desktop/prism-design-iteration/for-hyunmi.md` — 5/8 정리본 (현미 검토용)

---

*End of PRD v4 Core*
