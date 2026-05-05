# PRISM Provenance Layer — PRD v2.0 (Track 1.5 Operational)

*Korean Political Discourse Provenance Layer — Operational Spec for 2026-06 Local Election*

**Status**: Design locked (2026-05-05)
**Implementation start**: 2026-05-04 (in progress)
**Launch**: 2026-05-30 ~ 06-01 (선거 D-3 ~ D-5)
**First application**: 2026-06-03 지방선거
**Relation to v1**: v1.0 (`PRD.md`)의 strategic mission은 그대로 유지. v2는 그 위에 얹는 *operational layer*. v1의 F1~F5 framework는 v2 운영 결정에 따라 일부 재해석됨 (특히 F3 gate, F5 dashboard view).

---

## 0. 이 문서가 v1과 다른 이유
*Why This Document Replaces the Operational Layer of v1*

v1.0은 2026-04-19 시점에 "솔로 + AI automation 중심"으로 lock 되어 있었다. 이후 5명이 합류하여 6명 팀이 되었고, 운영 모델이 본질적으로 바뀌었다. 또한 4월 말 ~ 5월 초 사용자 진입장벽 / 차별화 / 법적 risk 에 대한 깊은 재검토가 진행되어 다음 6개 결정이 새로 내려졌다.

| 항목 | v1.0 | v2.0 |
|---|---|---|
| 실행 주체 | Jack 단독 + Codex 자동화 | 6명 팀 + Jack daily 직접 개입 |
| Cadence | 주 2회 파이프라인 업데이트 | Daily intensive (월~금 5장, D-7~D-1 escalation) |
| Output 형태 | 주간 Top-N 이벤트 대시보드 | Daily 카드 (이슈 라벨 + 출처 병렬) |
| 분류/판단 | "왜곡됨" 등 라벨 가능 | **Show, don't classify** — 분류 라벨 전면 금지 |
| Gate 형식 | LLM-human agreement ≥95% recursive | **Publish 전 1명 human approver 명시적 approval 필수** |
| Framing | 정치인·이벤트 단위 | **이슈 단위** (주거/노동/AI 규제/교육/안보 등) |

v1과 v2의 충돌이 있을 경우 **v2가 우선**한다 (운영 결정이 더 최근).

---

## 1. 미션과 우리가 아닌 것
*Mission & What We Are Not*

### 1.1 미션 (v1에서 carry, 변경 없음)
*Mission Statement (Carried from v1, Unchanged)*

한국의 정치 정보 환경은 구조적으로 극단화되어 있다. 지역 정서와 정당 충성도가 논리적 판단보다 우선하고, 같은 사실을 두고도 해석과 수용이 당파적으로 갈라진다. 이 구조 위에서 YouTube는 한국 정치 담론의 가장 큰 시장이 되었고, 그 안에서 왜곡된 주장·출처 불명의 루머·클릭베이트가 빠르게 확산되고 있다. 그러나 어떤 콘텐츠가 어디에서 시작되어 어떻게 퍼지는지 체계적으로 확인할 수 있는 공공 인프라는 존재하지 않는다. 2026년 6월 지방선거는 이 공백이 실제 투표 행동에 영향을 미치는 고위험 구간이다.

**PRISM 한 줄 정의 (v2 추가):**
> 한국 정치 YouTube에서 확산되는 클레임의 출처를 추적해, 사용자가 스스로 판단할 수 있도록 정보를 보여주는 도구.

### 1.2 우리가 아닌 것 (5가지)
*What We Are Not (Five Negative Definitions)*

PRISM의 정체성을 보호하는 가장 중요한 한 페이지. 6명 팀이 흔들리지 않게 하기 위한 governance lock이다.

1. **사회·경제 zeitgeist 도구가 아니다.** 한국 사회 전반의 핫뉴스를 정리하는 일반 큐레이션 서비스가 아니다. 사회·경제 lived experience와 *연결되는* 정치 클레임만 다루며, 정치와 무관한 사회·경제 이슈 자체는 다루지 않는다. (예: SNL Korea 풍자 사회현상 정리 — out of scope.)

2. **후보 평가 도구가 아니다.** "이 후보가 좋다 / 나쁘다" 판단을 사용자에게 제공하지 않는다. 후보 발언의 출처를 추적하고 관련 canonical source를 병렬 제시할 뿐, 후보 자체에 대한 정성적 평가는 하지 않는다.

3. **팩트체크 도구가 아니다.** "이 발언은 거짓이다" 라벨링을 하지 않는다. SNU팩트체크·JTBC팩트체크·뉴스타파와 다른 layer에 위치한다 (구체적 차별화는 §6 참고).

4. **투표 추천 도구가 아니다.** "어느 후보를 뽑으세요" 또는 그에 준하는 nudge를 일체 제공하지 않는다. 사용자의 투표 결정은 사용자의 영역이다.

5. **분류 도구가 아니다 (Show, don't classify).** "왜곡됨", "오해 소지", "True", "Misleading" 등 어떠한 평가 라벨도 카드에 부여하지 않는다. 출처와 데이터를 병렬로 보여줄 뿐, 분류는 사용자의 인지 영역에 남긴다. (이 원칙은 §4.2에서 구체화.)

이 5가지는 PRD-v2의 governance lock이다. 운영 중 이 lock을 흔드는 외부 압력 (예: "팩트체크 라벨도 추가하자", "투표 가이드도 만들자")이 들어오면 PRD 본문 §1.2를 reference로 거절한다.

---

## 2. 타겟과 범위
*Target Users & Scope*

### 2.1 1차 타겟
*Primary Target*

**정치 관심 minority** (전체 유권자의 5-15% 추정).
- 이미 정치 YouTube를 소비하고 있고 misinformation 신호에 민감한 사용자
- "이 영상이 어디서 시작된 이야기인지" 확인하고 싶은 동기가 있는 사용자
- 카톡으로 정치 콘텐츠를 친구·가족과 공유하는 습관이 있는 사용자

이 minority가 자발적 사용 + 공유를 시작하면 → 선거 D-7 ~ D-1 구간에 mass로 자연 확장.
**처음부터 mass를 노리지 않는다.** 처음부터 mass를 노린 정치 도구는 한국에서 거의 성공한 사례가 없다.

**언론·연구자 (2차)**: post-launch organic engagement 대상. 의도적으로 pre-launch 파트너십·outreach 없음.

### 2.2 콘텐츠 범위
*Content Scope*

**In Scope:**
- 한국 정치 YouTube 콘텐츠 (정치인·정당·정치 채널·정치 토론)
- 정치 클레임의 canonical source 추적 (§3)
- 이슈 단위 큐레이션 (§2.3)

**Out of Scope:**
- 사회·경제 zeitgeist 일반 (정치와 연결 안 되는 부분)
- TV 토론회 영상 분석, 외신, Substack·개인 블로그 (v1 그대로)
- ASR / 비디오 콘텐츠 자체 분석 (v1 그대로, Track 2로 이연)
- 카톡봇, 브라우저 확장 (post-MVP)
- 정치인 중심 drill-down 뷰 (이슈 framing이 우선, §2.3)
- 자동 claim verification / 분류 라벨 (§4.2)

### 2.3 이슈 분류 체계
*Issue Taxonomy*

PRISM은 **이슈 단위**로 사용자에게 진입한다. 후보·정당·이벤트 단위가 아니다. 이는 정치 fatigue 사용자가 본인 삶과 직결된 이슈를 통해 정치 콘텐츠에 접근하도록 하기 위함이다.

**1차 이슈 카테고리 (6-8개, launch 시점):**
1. **주거** — 청년 무주택, 부동산 정책, 임대차 등
2. **노동** — 최저임금, 노동시간, 청년 일자리, 산재 등
3. **교육** — 입시 정책, 사교육, 대학 등록금 등
4. **AI 규제** — AI 정책, 데이터 규제, 디지털 노동 등
5. **의료·복지** — 건강보험, 노인 복지, 출산 정책 등
6. **안보·외교** — 한미관계, 한일관계, 북한 등
7. **사법·민주주의** — 검찰 개혁, 언론 자유, 선거 제도 등
8. **지역** — 수도권 집중, 지역 균형 등 (지방선거 특성 반영)

이슈 카테고리는 launch 후 사용 데이터 기반으로 조정 가능. **카테고리 자체가 fixed PRD가 아님 — taxonomy는 큐레이션 워크플로우에서 진화.**

**카드의 진입 라벨은 이슈 단위:**
- 좋은 예: "청년 무주택 — 이번 주 정치권 인용 통계와 원본"
- 나쁜 예: "후보 X 노동 정책 발언 출처 추적"

같은 데이터, 다른 진입점. 이슈 framing이 비정치 사용자도 클릭할 수 있게 한다.

---

## 3. 데이터와 출처
*Data Sources & Canonical Source Graph*

### 3.1 Canonical Source 7종 (v1에서 carry)
*Seven Canonical Sources (Carried from v1)*

PRISM이 신뢰하는 1차 출처. 모든 카드는 이 7종 중 하나 이상을 출처로 인용해야 한다.

1. **정치인 공식 채널** (성명·보도자료)
2. **정당 공식 계정·보도자료**
3. **주요 언론 직접인용 기사** (조선·중앙·동아·한겨레·경향 + 연합·뉴시스 wire)
4. **정치인·정당 공식 SNS** (X, Facebook, Instagram — verified 계정만)
5. **네이버 블로그** (정치인 본인 운영)
6. **국회 속기록** (접근 가능 범위)
7. **정부·대통령실·부처 공식 발표** (정책브리핑 등)

**Out of canonical (인용 불가):**
- TV 토론회 영상 (별도 verification 필요)
- 외신 (한국어 원문 우선)
- Substack·개인 블로그
- 익명 게시판 (디시·일베·뽐뿌 등)
- 검증되지 않은 커뮤니티 캡처

### 3.2 Canonical Source Graph (v2 핵심 추가)
*Pre-Built Canonical Source Graph (New in v2)*

PRISM의 가장 중요한 차별화 자산. 사후 lookup이 아니라 **사전 매핑된 graph**로 구축한다.

**Graph 구성:**
- **Nodes**: 후보자 / 정당 / 정부 부처 / 국회 / 주요 언론 / 공식 SNS 계정 / 정치인 블로그 / 공식 정책 발표 채널
- **Edges**: 누가 누구의 account를 운영하는지, 누가 어느 정당 소속인지, 어느 부처가 어느 정책을 발표하는지, 어느 언론이 어느 후보를 어떻게 보도했는지

**Pre-loaded data:**
- 선관위 후보자 등록 정보 (2026 지방선거 후보자 전수)
- 후보자별 공식 SNS 계정 매핑 (manual + verification)
- 정부 정책 발표 archive (정책브리핑 RSS)
- 정치인 블로그 RSS feed list
- 주요 언론 7개사 정치 카테고리 RSS

**작동 흐름:**
```
[정치 YouTube에서 클레임 surface]
        ↓
[Graph 조회 — 클레임에 등장하는 인물·기관 식별]
        ↓
[관련 노드의 canonical source data 즉시 lookup]
        ↓
[카드 본문에 병렬 제시]
```

**Why this matters:** ChatGPT는 graph가 없고 web search만 한다 — 시간 걸리고 부정확. Snopes/뉴스타파는 사후 manual fact-check. PRISM은 *pre-built graph + real-time lookup + provenance UI*로 한국 정치 담론의 index 역할을 한다.

**기술 선택:** lightweight graph (PostgreSQL relations 또는 Neo4j community edition). 담당자 (정아인 + 김재훈) 합류 시 구체화.

### 3.3 BIGKinds 무료 웹 input signal (v2 추가)
*BIGKinds Free Web as Input Signal (New in v2)*

한국언론진흥재단 운영 BIGKinds (https://www.bigkinds.or.kr) 무료 웹 인터페이스를 큐레이션 input signal로 사용한다.

**활용 항목:**
- "오늘의 이슈" (1일 2회 클러스터링) — 이번 주 정치 카테고리 누적 review
- "오늘의 키워드" (3시간 간격 개체명 분석) — 인물·기관 트렌드 review

**활용 방식:**
- 큐레이터가 매주 월요일 + 매일 아침 BIGKinds 웹 접속, 정치 카테고리 review
- 추출된 이슈·키워드를 정치 YouTube monitoring과 cross-reference
- 큐레이션 input only — 사용자에게 BIGKinds 데이터 직접 surface 안 함

**활용 안 함:**
- BIGKinds API 구매 안 함 (Track 1.5 단계)
- 본문 텍스트 수집·저장 안 함
- LLM 컨텍스트에 BIGKinds 본문 주입 안 함 (약관 회피)

**Why 무료 웹으로 충분한가:** PRISM은 카드 5장/일 produce. BIGKinds 분석 빈도 (이슈 1일 2회 + 키워드 1일 9회)는 이 cadence에 충분. 자동화 안 해도 manual review가 quality 측면에서 더 우월.

**Phase 2 (post-launch) 검토:** BIGKinds API 학술용 50% 할인 신청 + 비영리 분류 검토. AI 활용 약관 (RAG·학습용 금지) 회피 architecture 설계.

### 3.4 정치 YouTube monitoring
*Political YouTube Monitoring*

v1의 F1 (YouTube 데이터 파이프라인)을 v2 운영에 맞게 재정의:

**Discovery:**
- 시드 채널 40-60개 (한국 정치 YouTube 주요 채널 — 좌·우 균형)
- YouTube 트렌딩 정치 카테고리 일일 review
- 후보자명 쿼리로 누락 보강

**수집 신호 (v1 22개 신호 carry):**
- API: 제목, 설명, 태그, duration, 조회수, 업로더, 업로드 시각, 채널 메타, Community tab, 댓글 (top + newest + pinned), topicCategories, 트렌딩 진입, 자막
- 파싱: Description URL·chapters
- AI 파생: 썸네일 OCR, 썸네일 패턴, 감성, 클릭베이트 탐지, 댓글 봇 탐지

**v2 추가 운영 layer:**
- 일일 trending claims 자동 추출 (LLM assist) → 큐레이터 검토 큐
- Canonical Source Graph 자동 lookup → 큐레이터에게 매칭 후보 제시
- 큐레이터가 카드 5장 선정 + 작성 + approve

**기술 stack 결정 (담당자 위임):** YouTube Data API v3 + yt-dlp 백업, Python 큐레이션 백엔드. 김재훈이 합류 시 구체화.

---

## 4. 코어 제품
*Core Product*

### 4.1 카드 form factor
*Card Form Factor*

PRISM의 사용자 facing primary unit은 **카드**. 카드는 모바일 우선 디자인이며, 카톡·X 공유에 최적화된다.

**카드 구성요소 (필수 8개):**

1. **이슈 라벨** (예: "주거", "노동", "AI 규제") — 진입 식별자
2. **카드 제목** — 이슈 + 한 줄 핵심 (예: "청년 무주택 — 이번 주 정치권 인용 통계와 원본")
3. **본문 (200-400자)** — 어떤 클레임이 어떻게 돌고 있는지 factual 기술
4. **클레임 병렬 제시** — 정치 YouTube에서 발견된 2-4개 클레임을 발화자·발화일·발언과 함께 나열
5. **Canonical source 병렬 제시** — 관련 통계청·부처·후보 공식 발표 등 1-3개 출처와 함께 나열
6. **출처 링크 (전부)** — 각 클레임의 영상 링크 + 각 canonical source의 원본 링크
7. **Provenance footer** — "PRISM은 발언을 분류하지 않습니다. 출처를 보여드립니다. 판단은 사용자 본인의 몫입니다."
8. **공유 버튼** — 카톡, X, 링크 복사

**카드 form factor 예시:**

```
[이슈 라벨: 주거]

청년 무주택 — 이번 주 정치권 인용 통계와 원본

이번 주 정치 YouTube에서 청년 무주택 통계 인용이
다음 세 가지 형태로 확산되었습니다.

[클레임]
- 후보 X (4/27 영상): "청년 50% 무주택"
- 후보 Y (4/28 토론회): "청년 38% 무주택"
- 정치 채널 Z (4/29 영상): "청년 무주택 60% 시대"

[관련 canonical source]
- 통계청 2025 인구주택총조사: 청년 35% 무주택
- 국토부 2025 보고서: 19~34세 무주택 38%

[출처]
- 후보 X 영상 [링크]
- 후보 Y 발언 [링크]  
- 정치 채널 Z 영상 [링크]
- 통계청 원본 [통계청.kr]
- 국토부 보고서 [molit.go.kr]

PRISM은 발언을 분류하지 않습니다.
출처를 보여드립니다.
판단은 사용자 본인의 몫입니다.

[카톡 공유] [X 공유] [링크 복사]
```

**카드 톤 가이드라인:**
- 표준 존댓말 ("~합니다", "~입니다")
- Factual + 약간의 인간미 (캐릭터 voice X, 위트 X, 풍자 X)
- 정치 평가 표현 금지 ("우려된다", "주목된다", "심각하다" 등)
- 형용사·부사 최소화 (객관성 보호)

### 4.2 "Show, don't classify" 원칙
*The "Show, Don't Classify" Principle*

PRD-v2의 가장 중요한 product 결정. PRISM은 어떠한 평가 라벨도 부여하지 않는다.

**금지되는 표현:**
- ❌ "왜곡됨", "거짓", "오해 소지", "맥락 부족"
- ❌ "True", "False", "Misleading", "Mostly True"
- ❌ "사실과 다름", "검증됨"
- ❌ "주의 필요", "확인 요망"

**허용되는 표현:**
- ✅ "다음과 같은 통계가 있습니다"
- ✅ "통계청 자료에 따르면"
- ✅ "원본을 확인할 수 있는 자료는 다음과 같습니다"
- ✅ "관련 정부 발표는 다음과 같습니다"

**이 원칙이 풀어주는 3가지:**

1. **법적 risk 최소화.** 분류하지 않으면 명예훼손·허위사실공표 노출이 거의 0이 된다. 한국 명예훼손법 + 공직선거법 250조 (후보자 비방) + 82조의4 (허위사실공표) 모두 *판단·평가 진술*에 적용된다. 사실 병렬 제시는 적용 대상 아님.

2. **사용자 자율성 존중.** 가르치는 톤이 0이 된다. PRISM은 정보를 보여줄 뿐, 결론을 강요하지 않는다. 이는 한국 사용자가 정치 라벨링에 갖는 거부감 (좌좀·우꼴 트라우마)과 정확히 반대 방향.

3. **운영 부담 감소.** "이 발언이 X 카테고리에 해당하는가" 판정 부담이 사라진다. 큐레이터는 사실 수집 + 출처 매핑 + 병렬 제시만 하면 된다. Daily intensive cadence 가능.

**원칙 위반 감지 mechanism:**
- 모든 카드는 publish 전 human approver review (§5.2)
- Approver checklist에 "분류 라벨 포함 여부" 항목 명시
- 위반 시 publish 거부, 큐레이터에게 reword 요청

### 4.3 Provenance UI surface
*Provenance UI Surface*

v1 §F4 KG ontology의 사용자 facing UI 형태.

**카드 안 provenance 시각화:**
- 각 클레임에 "발화자 / 발화일 / 영상 위치" 메타데이터 visible
- 각 canonical source에 "출처 기관 / 발표일 / 원본 URL" visible
- 클레임과 canonical source가 *충돌*할 때, 양쪽을 평등하게 병렬 제시 (어느 쪽이 맞는지 판단 안 함)

**확장 view (post-launch 검토):**
- 클레임 graph view (이 클레임이 어디서 어디로 퍼졌나)
- 시간 축 view (이 이슈가 어떻게 진화했나)
- 채널 cluster view (어느 진영의 어떤 채널이 다뤘나)

**Track 1.5 launch는 카드 view 단일.** 그래프·시간축·클러스터 view는 post-launch 검토 항목.

### 4.4 카톡 공유 최적화
*KakaoTalk Sharing Optimization*

한국에서 정치 콘텐츠 viral의 primary path는 카톡 공유. PRISM은 카톡 공유 mechanic을 first-class citizen으로 design.

**카톡 미리보기 최적화:**
- Open Graph 메타데이터: 카드 제목, 첫 80자 본문, 이슈 라벨 색상 thumbnail
- 미리보기에서 핵심 정보 (제목 + 이슈 + "출처 X개") 즉시 visible
- "PRISM" 로고 작게 워터마크 (브랜드 인지)

**공유 link 구조:**
- 각 카드는 unique URL (`prism.kr/cards/{date}-{slug}`)
- 공유 link 클릭 시 → 카드 상세 → "다른 카드 보기" CTA → 사이트 진입

**Drop schedule + 공유 trigger:**
- 매일 평일 오전 8시 카드 드롭 (출근/등교 시간)
- 매일 카드 5장 중 하나는 "공유 가능성 높은" 이슈로 의도적 배치 (큐레이터 판단)

**Why 카톡 우선:** 한국 사용자의 정치 콘텐츠 공유는 X (5%) < Facebook (10%) < 카톡 (60%). 카톡 미리보기 미최적화 = viral 자살.

---

## 5. 운영
*Operations*

### 5.1 Daily curation workflow
*Daily Curation Workflow*

**Cadence:**
- **월~금**: 매일 카드 5장 publish
- **주말**: 카드 1-2장 또는 휴식 (선택적)
- **D-7 ~ D-1 (5/27 ~ 6/2)**: 매일 카드 7-10장 escalation
- **D-Day (6/3)**: 별도 운영 (실시간 monitoring + 사후 분석)
- **D+1 이후**: 일일 카드 3장 정리 + 학술 정리 착수

**일일 워크플로우:**

```
오전 8:00  큐레이터 lead (Jack) BIGKinds + YouTube monitoring 결과 review
오전 8:30  AI assist로 trending claims 자동 추출 → 큐레이터 검토 큐
오전 9:00  큐레이터 lead가 그날 카드 후보 10-15개 선정
오전 9:30  AI assist로 카드 초안 자동 작성 (claim + canonical source lookup)
오전 10:00 큐레이터 lead가 5장 final 선정 + 인간미·톤 가다듬기
오전 11:00 다른 팀원 1명이 publish gate review (§5.2)
정오 12:00 5장 publish + 카톡 공유 trigger
오후       사용자 반응·공유 monitoring + 다음날 준비
```

**총 부담:**
- Lead 큐레이터 (Jack): ~3시간/일
- Reviewer (다른 팀원 1명, rotation): ~30분/일
- 백엔드·디자인 (김재훈·김현미·Saetbyeol): on-call + weekly improvement

### 5.2 1 human approver gate (publish lock)
*Single Human Approver Gate*

PRD-v2의 second load-bearing pillar. v1의 "≥95% recursive gate"의 운영 가능 형태.

**Gate 정의:**
> PRISM card는 최소 1명의 human approver의 명시적 approval 없이 publish 되지 않는다.

**Approver checklist (publish 전 review 항목):**
1. 클레임 원본 영상 link 작동 여부 + 발언 정확성
2. Canonical source 원본 link 작동 여부 + 인용 정확성
3. 분류 라벨 포함 여부 (§4.2 위반 감지)
4. 정치 평가 표현 포함 여부 (§4.1 톤 가이드라인 위반 감지)
5. 후보 평가·투표 추천 함의 포함 여부
6. 명예훼손 위험 평가 (특정인을 부정적으로 묘사하는 표현 없는지)

체크리스트 6개 모두 통과 + approver 명시적 approval → publish.
하나라도 미통과 → 큐레이터에게 reword 요청, re-review.

**Approver rotation:**
- 6명 팀에서 rotation. Lead 큐레이터 (Jack)가 작성한 카드는 Jack 외 1명이 review.
- Approver 본인이 작성한 카드는 본인이 review 못 함 (self-approval 금지).
- Approver 이름은 카드 metadata에 기록 (사후 추적 가능, 사용자 facing 안 함).

**Gate 실패 시 escalation:**
- 큐레이터-approver 사이 의견 충돌 → 팀 group chat으로 escalation
- 합의 못 하면 publish 보류 (그날 카드 4장 또는 3장으로)
- "publish 못 한 카드"가 누적되면 weekly review에서 패턴 분석

**왜 1명인가, ≥95% 합의가 아닌가:**
v1의 ≥95% recursive gate는 LLM-human agreement에 대한 기술적 metric이었다. 운영 단계에서는 *publish 책임자가 명확히 1명* 있는 것이 더 안전 + 더 빠르다. 5명 합의를 매 카드마다 추구하면 daily cadence 불가능. 1명 명시적 approval은 daily 가능하면서도 "사람이 책임진다"는 ethical/legal 보호 유지.

### 5.3 AI 활용 layer
*AI Utilization Layer*

PRISM은 AI를 적극 활용한다. 단, **publish 결정은 안 한다.**

**AI 자동화 항목:**
- BIGKinds 웹 review 보조 (요약·분류)
- YouTube trending claims 자동 추출
- Canonical Source Graph 자동 lookup
- 카드 초안 자동 작성 (claim + source 병렬 텍스트 생성)
- 톤·스타일 일관성 check
- 분류 라벨 포함 여부 자동 감지 (§4.2 위반 sentinel)
- 카톡 미리보기 메타데이터 자동 생성

**AI 자동화 안 하는 항목 (사람이 함):**
- 그날 publish할 5장 final 선정
- 카드 톤·인간미 최종 가다듬기
- Publish gate approval (§5.2)
- Risk 판정 (명예훼손·정치 평가)

**AI model 선택 (담당자 위임):**
- Claim 추출·요약: GPT-4 / Claude / Gemini 등 (cost·quality 비교)
- Embedding (claim clustering): KoSimCSE 또는 다국어
- Korean NER: BERT 한국어 fine-tuned (v1 그대로)

**AI 비용 budget:**
- Track 1.5 launch 기간 (5/4 ~ 6/3 = 30일): 월 ~$200-500 USD 예상
- Jack 본인 부담 또는 SIPA stipend (확인 필요)

### 5.4 6명 역할 분담
*Six-Person Team Allocation*

| 역할 | 담당 | 일일 부담 |
|---|---|---|
| Lead + 큐레이션 큐레이터 | Jack | ~3시간/일 (큐레이션 lead) |
| Backend / pipeline | 김재훈 | Week 1-2 풀타임, launch 후 on-call |
| Governance / 거버넌스 + 이슈 큐레이션 보조 | 정아인 | Week 1-2 풀타임, launch 후 ~1시간/일 (approver rotation 포함) |
| 디자인 / UX | 김현미 | Week 1-2 풀타임, launch 후 ~30분/일 |
| 디자인 / Provenance UI | Saetbyeol Leeyouk (MIT Media Lab, 합류 확정 5/4) | Week 1-2 풀타임, launch 후 weekly review |
| 6번째 멤버 | TBD (recruitment 진행 중) | 합류 시 역할 정의 |

**Jack daily commitment (PRD에 명시):**
> Jack은 launch 기간 (5/4 ~ 6/3) 매일 lead 큐레이션을 직접 담당하며, daily cadence + 카드 quality + 분류 라벨 위반 감지에 직접 책임을 진다. 본인 학업 일정과 충돌 시 사전 공지 + 백업 큐레이터 (정아인) 지정.

**Approver rotation schedule:**
- 월요일: 정아인
- 화요일: 김현미
- 수요일: 김재훈
- 목요일: Saetbyeol
- 금요일: Jack (Jack이 큐레이션 안 한 카드만)
- 주말: 정아인 (선택적)

---

## 6. 차별화와 포지셔닝
*Differentiation & Positioning*

### 6.1 경쟁 도구 대비 차별화
*Competitive Differentiation*

| 도구 | 강점 | PRISM 대비 약점 |
|---|---|---|
| **ChatGPT / Perplexity / Claude** | Universal Q&A, 빠름, 무료/저렴 | Pre-built Korean political graph 없음. Real-time YouTube provenance 못 함. Hallucination risk. |
| **뉴닉 / 슬로우뉴스 / 피렌체의 식탁** | 정리 잘된 daily news, 가벼운 톤 | 클레임 단위 추적 안 함. Provenance 표시 없음. 사회 일반. |
| **SNU팩트체크 / JTBC팩트체크 / 뉴스타파** | 깊은 fact-check, 신뢰성 | 라벨링 (True/False) 적용. 사후 + 느림. YouTube ecosystem 분석 안 함. |
| **Snopes / Ground News (영문)** | 글로벌 모델 | 한국어 정치 이해 없음. Korean canonical source 없음. |

**PRISM의 unique 위치:**
> 한국 정치 YouTube의 ecosystem-level provenance를, 분류 없이, daily cadence로, canonical source graph로 제공하는 유일한 도구.

### 6.2 "PRISM은 LLM 단독 판단 안 합니다" surface
*Ethical Lock as User-Facing Message*

PRD-v2의 marketing/positioning 한 줄. 모든 사용자 facing 페이지에 명시.

**Surface 위치:**
- 사이트 footer
- About 페이지
- 각 카드 footer (간략 버전: "사람이 검수했습니다")
- 카톡 공유 미리보기 description

**구체 문구 (한국어):**
> PRISM은 모든 카드를 사람이 직접 검수한 후 공개합니다. AI는 카드 작성을 보조하지만, 어떤 카드가 공개될지는 사람이 결정합니다. 우리는 출처를 보여드릴 뿐, 발언을 분류하지 않습니다.

이 한 단락이 PRISM의 정체성 lock + 차별화 lock + 신뢰 lock 동시 수행.

### 6.3 한국어 정치 YouTube provenance white space
*Korean Political YouTube Provenance White Space*

한국에 prior art가 거의 없음. 가장 가까운 시도들:
- SNU팩트체크: 라벨링 + 사후 + 사이트 시각화 약함
- JTBC팩트체크: 방송 형식 + scale 작음
- 뉴스타파: 탐사보도 형식 + 일회성

**PRISM이 채우는 white space:**
- Daily cadence (다른 도구는 weekly 또는 event-based)
- 클레임 단위 그래프 (다른 도구는 article 단위)
- 라벨 없음 (다른 도구는 라벨링 중심)
- YouTube ecosystem 직접 분석 (다른 도구는 텍스트 기사 중심)
- Canonical source graph 사전 매핑 (어떤 도구도 안 함)

이 white space가 Track 2 (2027 대선)의 더 큰 가치 제안의 sandbox.

---

## 7. 2주 Sprint 계획
*Two-Week Sprint Plan*

### 7.1 Week 1 (5/4 ~ 5/10) — Draft
*Week 1: Build Draft*

| 일자 | Jack | 김재훈 (BE) | 정아인 (거버넌스) | 김현미 (디자인) | Saetbyeol (UX) |
|---|---|---|---|---|---|
| 5/4 (월) | 팀 킥오프 + PRD-v2 walkthrough | 개발 환경 셋업 | Canonical Source Graph schema 설계 | 카드 form factor wireframe | 합류 + onboarding |
| 5/5 (화) | 이슈 taxonomy 1차 lock | Pipeline backbone 시작 | 후보자 등록 정보 수집 (선관위) | Wireframe v1 완성 | Provenance UI 컨셉 |
| 5/6 (수) | 시드 채널 list 40-60 정의 | YouTube API 연동 | 후보자 SNS 매핑 시작 | Visual identity 시안 | Provenance UI 시안 |
| 5/7 (목) | 큐레이션 워크플로우 dry-run | Canonical Source lookup API | SNS 매핑 50% | Card mockup v1 | Card visual integration |
| 5/8 (금) | First test card 큐레이션 | LLM assist drafting 통합 | 정부·언론 RSS 통합 | Card mockup v2 | Provenance UI v1 |
| 5/9 (토) | (휴식 권장) | (휴식 권장) | (휴식 권장) | (휴식 권장) | (휴식 권장) |
| 5/10 (일) | Week 1 retro + Week 2 plan | Backend MVP demo | Graph data 80% complete | Site mockup | Final UI integration |

**Week 1 milestone:** End of week에 카드 1장이 site에 publish 가능한 상태. End-to-end demo.

### 7.2 Week 2 (5/11 ~ 5/17) — Revision
*Week 2: Revise & Harden*

| 일자 | 활동 |
|---|---|
| 5/11 (월) | 큐레이션 워크플로우 실전 테스트 (5장 작성) |
| 5/12 (화) | Approver gate 실전 테스트 (rotation 첫 cycle) |
| 5/13 (수) | 카톡 공유 미리보기 최적화 + 사이트 performance |
| 5/14 (목) | Beta 사용자 5-10명 invite (팀 친구) + feedback 수집 |
| 5/15 (금) | Beta feedback 반영 + bug fix |
| 5/16 (토) | (휴식) |
| 5/17 (일) | Week 2 retro + Week 3 plan |

**Week 2 milestone:** End of week에 daily 5장 cadence 가능 상태 + beta 사용자 positive feedback.

### 7.3 Week 3 (5/18 ~ 5/24) — Soft Launch & Iteration
*Week 3: Soft Launch*

- 5/18 ~ 5/22: Soft launch (limited audience, 친구·동료 위주 공유). Daily 5장 운영. 이슈 수정.
- 5/23 ~ 5/24: Pre-launch final check. Legal review (한국 변호사 자문 — 현미·아인이 섭외 lead).

### 7.4 Week 4 (5/25 ~ 5/31) — Public Launch
*Week 4: Public Launch*

- 5/25 ~ 5/29: Daily 5장 운영, 사용자 baseline 수집.
- **5/30 (토)**: 공식 launch. 카톡·X 공유 push.
- 5/31 (일): Launch day monitoring.

### 7.5 Election Week (6/1 ~ 6/3) — Escalation
*Election Week: Escalation Mode*

- 6/1 (월) ~ 6/2 (화): Daily 7-10장 escalation cadence.
- **6/3 (수) 선거일**: 실시간 monitoring + 사후 정리. Daily 5장 + 추가 event-driven 카드.

### 7.6 Launch readiness checklist
*Launch Readiness Checklist*

Public launch (5/30) 전 필수 통과 항목:

- [ ] Daily 5장 cadence 1주 연속 안정 운영
- [ ] Approver gate 모든 카드 적용 + checklist 6개 통과율 100%
- [ ] 분류 라벨 위반 0건 (지난 1주)
- [ ] 카톡 미리보기 정상 작동 (5개 device 테스트)
- [ ] Canonical Source Graph 후보자 매핑 ≥80% 완성
- [ ] BIGKinds 웹 review 워크플로우 정착
- [ ] 한국 변호사 preliminary check 완료
- [ ] About 페이지 + "PRISM은 LLM 단독 판단 안 합니다" surface 노출
- [ ] 6명 팀 burn-out 신호 monitoring (weekly retro)

---

## Appendix A. 결정 reasoning trail
*Decision Reasoning Trail*

각 핵심 결정의 *왜*. Future Jack 또는 신규 합류자가 "왜 이렇게 했는가"를 추적할 수 있게 하는 reference.

### A.1 왜 daily intensive (weekly 아님)
2026-05-05 세션에서 결정. 초기에는 weekly drop 제안되었으나 Jack이 "사용자에게 큰 매력이 없을 것 같다, 매일 업데이트가 필요하다"고 push back. Daily가 retention + 카톡 공유 trigger + 선거 시즌 attention curve에 맞다고 판단. 운영 부담은 AI assist drafting + parallelization + 1 human approver gate로 daily 가능 수준 유지.

### A.2 왜 Show, don't classify (라벨링 아님)
2026-05-05 세션. 제안 단계에서 "통계 왜곡으로 분류됨" 같은 라벨이 카드에 나오는 시나리오가 등장했고, Jack이 "이렇게까지 얘기하는 건 조금 문제가 있다"고 즉각 거부. 법적 risk (명예훼손 + 공직선거법 250조 후보자 비방 + 82조의4 허위사실공표)가 모두 *판단·평가 진술*에 적용된다는 점을 고려할 때, 분류 자체를 안 하는 것이 가장 안전 + 차별화 강함 + 사용자 자율성 존중. 한국 팩트체크 도구들 (SNU팩트체크 등)이 모두 라벨링하는 가운데 PRISM이 안 한다는 것이 white space.

### A.3 왜 1 human approver gate (≥95% 합의 아님)
v1의 ≥95% recursive gate는 기술 metric (LLM-human agreement). 운영 단계에서는 *publish 책임자가 명확히 1명*인 것이 더 안전·빠름. 5명 합의를 매 카드마다 추구하면 daily 불가능. 1명 명시적 approval은 daily 가능 + "사람이 책임진다"는 legal/ethical 보호 유지. v1 gate concept는 폐기 아님 — operational 형태로 변환.

### A.4 왜 정치 YouTube only (사회/경제 zeitgeist 아님)
2026-05-05 세션에서 깊은 검토. 사회/경제 zeitgeist 통합 모델이 한 차례 제안되었고 Jack이 거의 동의했으나, Jack 본인이 (1) 시간 escalation 구조 변화 risk, (2) 콘텐츠 범위 dilution, (3) News API + YouTube cross-correlation 기술 부담을 짚어 거부. Mission lock이 정치 YouTube misinformation provenance인 이상 scope을 그것에 단단히 묶는 것이 옳다. 사회/경제는 *콘텐츠 범위*가 아니라 *framing 언어*로만 들어옴 (이슈 taxonomy).

### A.5 왜 이슈 framing (후보 framing 아님)
한국 사용자가 일상에서 "정치/경제/사회"보다 "주거", "노동" 같은 이슈로 정보를 분류한다는 mental model 가설. 후보 framing은 정치 fatigue 사용자 즉시 이탈시키지만, 이슈 framing은 정치에 관심 없어도 본인 삶과 직결되니 클릭 유도. 같은 데이터, 다른 진입점.

### A.6 왜 BIGKinds 무료 웹 (API 구매 아님)
2026-05-05 세션. BIGKinds API 견적서 검토 결과 (1) "AI용(학습용·파인튜닝용·RAG용) 활용 불가" 약관이 PRISM의 LLM 활용과 충돌, (2) 견적 요청 → 응답 사이클이 1-3주로 2주 빌드 timeline에 위험, (3) 본문 200자 제한도 큐레이션에 충분치 않은 측면. 무료 웹은 "오늘의 이슈" 1일 2회 + "오늘의 키워드" 3시간 간격 제공으로 weekly review 충분. Phase 2 (post-launch)에서 API 학술 할인 + AI 격리 architecture로 검토.

---

## Appendix B. 거부된 대안
*Rejected Alternatives*

PRD-v2 작성 과정에서 진지하게 검토되었으나 거부된 대안들. 향후 외부 압력으로 재제안될 가능성이 있어 명시적으로 기록.

### B.1 사회/경제 zeitgeist 통합 모델 (거부)
*Rejected: Society/Economy Zeitgeist Integration*

제안 형태: "이번 주 한국이 본 것" 카드 7장 — 정치 4 + 사회/경제 3, 모든 카드에 PRISM provenance 처리.

거부 이유: Mission dilution. 6명 팀이 PRISM의 정치 misinformation mission에 모인 것이지 일반 zeitgeist 도구에 모인 것이 아님. Scope 확장 시 분석 대상 폭증, 큐레이션 burden 증가, 차별화 약화 (뉴닉·카카오뷰와 경쟁 진입). 사회/경제 element는 *콘텐츠*가 아니라 *framing*으로 들어옴.

재제안 시 응답: §1.2 ("사회·경제 zeitgeist 도구가 아니다") reference.

### B.2 분류 라벨 (True/False/Misleading) (거부)
*Rejected: Classification Labels*

제안 형태: 각 클레임에 "사실 / 부분 사실 / 사실과 다름" 등 라벨 부여.

거부 이유: §A.2 + 한국 명예훼손법 노출 + 차별화 자살 (한국 팩트체크 도구들과 동일 layer로 commoditize) + 사용자 자율성 침해.

재제안 시 응답: §1.2 ("분류 도구가 아니다") + §4.2 reference.

### B.3 Time-based UI escalation (거부)
*Rejected: Time-Based UI Escalation*

제안 형태: 선거 임박 시 사이트 layout 자체가 election-heavy로 자동 변환.

거부 이유: 사용자에게 product 구조 자체 변화는 confusion. NYT도 election week에 layout 안 바꾼다 — 헤드라인 weight만 바꾼다. PRISM도 structure stable, content escalation으로.

재제안 시 응답: §5.1 (cadence escalation은 카드 수만, 구조 변화 X) reference.

### B.4 BIGKinds API 구매 (Track 1.5에서 거부, Phase 2 검토)
*Rejected for Track 1.5: BIGKinds API Purchase*

§A.6 참고. Track 1.5 launch 안정화 후 검토.

### B.5 50개 정치 채널 인지 부담 입력 (거부)
*Rejected: 50-Channel Recognition Quiz Onboarding*

제안 형태: 사용자가 처음 진입 시 "이 50개 정치 YouTube 채널 중 본 것 체크" → persona detection.

거부 이유: 인지 부담 과다 + 정치 채널 50개에 관심 자체 없는 사용자 다수 + persona detection의 felt value 불명확 + cold-start 진입장벽 자살.

재제안 시 응답: §2.1 (1차 타겟은 자발 진입 minority, persona 추론 안 함) reference.

### B.6 캐릭터 voice / 위트 / 풍자 톤 (거부)
*Rejected: Character Voice / Wit / Satire Tone*

제안 형태: 뉴닉 같은 캐릭터 voice ("~했어요!", "~하답니다") 또는 SNL Korea 같은 위트.

거부 이유: 진중·신뢰 톤이 PRISM positioning에 더 맞음. 캐릭터 voice는 trustworthy 인상 약화. SNL 위트는 정치 평가 함의 risk + 객관성 손상.

재제안 시 응답: §4.1 (표준 존댓말 + factual + 인간미) reference.

---

## Appendix C. Risks
*Risks*

### C.1 명예훼손 / 공직선거법 risk
- **Risk**: 카드 내 발언 인용 부정확 → 명예훼손. 또는 carrying claim about candidate → 후보자 비방 (공직선거법 250조).
- **Mitigation**:
  - §4.2 분류 라벨 금지 (가장 큰 mitigation)
  - §5.2 1 human approver gate + checklist (#6 명예훼손 위험 평가)
  - Week 3 한국 변호사 preliminary check
  - 클레임 인용 시 원본 영상 link + timestamp 명시 (사용자가 직접 verify 가능)
  - 카드 publish 후 issue 발견 시 즉시 retraction protocol (24시간 내 takedown)

### C.2 운영 burn-out risk
- **Risk**: Daily intensive cadence + 6명 팀 + 학생 신분 → burn-out. Election D-7~D-1 escalation 구간 위험.
- **Mitigation**:
  - Approver rotation (한 명에게 부담 집중 안 됨)
  - 주말 light cadence (선택적)
  - Weekly retro에서 burn-out 신호 monitoring
  - Jack daily commitment + 백업 큐레이터 (정아인) 사전 지정
  - Election week 후 mandatory rest week (6/4 ~ 6/10)

### C.3 Scope creep risk
- **Risk**: 사용 후 사용자·팀원·외부에서 "사회·경제도 다루자", "팩트체크 라벨도 넣자" 등 압력.
- **Mitigation**:
  - §1.2 "What we are not" 5가지를 PRD 본문에 명시 (governance lock)
  - Appendix B "Rejected alternatives"에 거부 이유 기록 (재제안 응답 자료)
  - Weekly retro에서 scope creep 시그널 review

### C.4 차별화 erosion risk
- **Risk**: 운영 부담 압력 → "AI 자동 publish 일부 허용", "분류 라벨 일부 도입" 등 양보 압력.
- **Mitigation**:
  - §6.2 "PRISM은 LLM 단독 판단 안 합니다"가 PRD 본문 + 사용자 facing 모두 lock
  - 본 PRD revision은 6명 팀 합의 + 명시적 PRD update 없이 변경 금지
  - Jack 본인 commitment: gate 약화 압력에 대한 1차 방어선

### C.5 한국어 엔티티·사실 추출 quality risk
- **Risk**: AI assist 단계에서 LLM이 한국어 정치 entity·통계를 잘못 추출 → 큐레이터 review에서 필터되지만, 누락 시 잘못된 카드 publish.
- **Mitigation**:
  - KR-WordNet + 선관위·국회 DB grounding (v1 그대로)
  - Canonical Source Graph로 entity verification (graph에 없는 entity는 큐레이터 manual check)
  - Approver checklist #1, #2 (원본 link 작동 + 인용 정확성)

### C.6 YouTube API quota / 차단 risk
- **Risk**: YouTube Data API v3 quota 한계 (기본 10K units/day) + 정치 콘텐츠 monitoring으로 빠른 소진.
- **Mitigation**:
  - 시드 채널 limit 40-60으로 설정
  - yt-dlp 백업 (API 차단 시)
  - 필요 시 quota 상향 신청

### C.7 6번째 멤버 미합류 risk
- **Risk**: 현재 5명 + 6번째 recruitment 진행 중. 미합류 시 6명 팀 가정한 부담 분배 흔들림.
- **Mitigation**:
  - 5명으로도 daily cadence 가능한 일별 부담 sanity check (§5.4 부담 ~30분/일/인 수준이라 5명도 가능)
  - 6번째 합류 시 역할 정의 후 부담 재분배

---

## Appendix D. Phase 2 / Track 2 핸드오프
*Phase 2 / Track 2 Handoff*

Track 1.5 launch (2026-06-03 이후) 검토 항목.

### D.1 BIGKinds API 검토
- 학술 50% 할인 신청 (Jack SIPA 학생 증빙)
- 비영리 분류 신청
- AI 활용 약관 회피 architecture: BIGKinds 데이터는 큐레이터 UI only, LLM 컨텍스트 격리

### D.2 자동화 확장
- AI assist drafting → publish 자동화 검토 (gate는 유지)
- Canonical Source Graph 자동 lookup 정확도 metric 측정 + 개선
- 카드 작성 자동화 quality benchmark

### D.3 Track 2 (2027 대선) 학습
- Track 1.5 사용 데이터 분석 (어떤 이슈·카드가 가장 공유됐나)
- 6명 팀 retro: daily intensive cadence 지속 가능성
- ASR / 비디오 콘텐츠 분석 추가 검토 (v1 deferred 항목)
- 정치인 drill-down 뷰 추가 검토 (v1 C 단계 확장)
- KG ontology 학술 정리 + 논문 발표 가능성

### D.4 사용자 경험 개선
- 카드 graph view (claim propagation 시각화)
- 시간 축 view (이슈 진화 추적)
- 채널 cluster view (진영별 다룬 채널)
- 검색 / 필터 기능 (이슈·기간·후보)
- 사용자 피드백 mechanism (제보 / 정정 요청)

### D.5 거버넌스 확장
- 한국 변호사 ongoing retainer 검토
- 외부 advisor council (학계·언론·시민사회) 구성 검토
- Bug bounty / public correction protocol

---

## Canonical References
*Canonical References*

- `PRD.md` (v1.0) — Strategic spec, 2026-04-19 lock. v2의 mission statement 출처.
- `archive/PLAN_v0_superseded.md` — Historical draft.
- `docs/deck.html` — Recruitment deck (Saetbyeol 섭외 자료).
- `docs/mock-dashboard.html` — Mock dashboard (recruitment 자료).
- `docs/recruitment-onepager.md` — Recruitment one-pager.
- `~/.claude/projects/-Users-jackkang/memory/project_prism-provenance.md` — Project status memory.
- `~/.claude/plans/snappy-coalescing-sparkle.md` — Previous lead-magnet exploration session.

---

## Document Metadata

**Version**: v2.0
**Last updated**: 2026-05-05
**Authors**: Jack Kang (강동재) + Claude (writing assist, all decisions Jack-led)
**Approval**: Pending Jack final review + 6명 팀 walkthrough on 2026-05-04 kickoff.
**Update protocol**: PRD-v2 본문 변경은 6명 팀 합의 + 명시적 commit message 필수. Appendix는 Jack 단독 update 가능.

---

*End of PRD v2.0*
