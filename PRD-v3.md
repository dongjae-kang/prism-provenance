# PRISM Provenance Layer — PRD v3.0 (Track 1.5 Operational, Flow Medium)

*Korean Political Discourse Provenance Layer — How Information Moves, Daily*

**Status**: Design locked (2026-05-05, post Jaehoon call + 현미 1차 인계 미팅)
**Implementation start**: 2026-05-04 (in progress)
**Launch target window**: 2026-05-30 ~ 06-01 (선거 D-3 ~ D-5)
**First application**: 2026-06-03 지방선거
**Relation to v2**: v2.0 (`PRD-v2.md`)의 운영 layer는 그대로 carry. v3은 (a) 5/5 [[김재훈]] 콜에서 결정된 데이터 소스·윤리·운영 변경, (b) 5/5 [[김현미]] 1차 인계 미팅 피드백을 반영한 self-positioning 강화 — 이 두 축의 통합본.

---

## 0. 이 문서가 v2와 다른 이유
*Why This Document Replaces v2*

v2.0은 2026-05-05 새벽 (NYC) 솔로 작성으로 lock 되어 있었다. 같은 날 아침 (NYC 01:24~02:53 = KST 14:24~15:53)에 [[김재훈]]과 1.5시간 콜, 같은 날 오후 (KST 16:00 = NYC 03:00) [[김현미]]와 1차 PRD 인계 미팅이 진행되었고, 그 결과 다음 결정들이 새로 추가되었다.

| 항목 | v2.0 | v3.0 |
|---|---|---|
| 메인 데이터 소스 (보조) | BIGKinds 무료 웹 review | **`data.go.kr` (공공데이터포털) 무료 API** — 국회 속기록·정책 브리핑·선거 정보지 |
| 윤리 layer | "Show, don't classify" 원칙만 | **Show-don't-classify + Process Transparency Disclosure 페이지 (인간 개입 절차 공개)** |
| Self-positioning | "분류하지 않는 출처 도구" | **"흐름 매체" (how-it-moved)** — 정보가 어떻게 변형되며 퍼졌나를 매일 다루는 한국 유일 매체 |
| 카드 form factor | 클레임 + canonical source 병렬 | **+ 시간 순서 + 출처 성격** 두 메타 줄 추가 |
| Cadence | 평일 daily 5장만 | 평일 daily 5장 + **토요일 Weekly Flow 1장** (그 주 가장 활발히 변형된 클레임 1개 심층 추적) |
| 6명 팀 | 모두 풀타임 가정의 일정 | **풀타임 가정 폐기** — 풀타임 멤버 없음. 토요일 작업 정상. 마일스톤만 명시 |
| Saetbyeol 합류 시점 | "5/4 합류 확정" (오류) | **5/11 합류** (정정) |
| Sam Lee (EPFL) | 미언급 | **개발 담당으로 합류 완료** (카톡방 합류, 5/11~ 본격 시작) |
| Product 형태 | 명시 안 함 | **Web** (앱 X, 반응형) |
| Lo-fi → Hi-fi | 별도 단계 | **Lo-fi 단계 단축** — Claude Code 데모를 그대로 lo-fi로 간주 |
| 8개 이슈 카테고리 | "fixed가 아니지만 launch 시점 lock" | **MECE 재고민 필요** — 사람들이 관심 가지는 사회 전반을 정말 잘 포함하는가 한 번 더 검토 |
| 차별화 표 | ChatGPT / 팩트체크 도구만 | **+ 뉴닉·슬로우뉴스 행 추가** + "흐름 매체" 위치 명시 |
| 6번째 멤버 | TBD | **샘 리(Sam Lee) 합류로 6명 확정** (5명 + 5/11~ Saetbyeol + Sam Lee) |

v2와 v3의 충돌이 있을 경우 **v3가 우선**. v1 (strategic spec)의 mission statement는 v3에서도 그대로 carry.

---

## 1. 미션 · 우리가 아닌 것 · 우리가 무엇인가
*Mission, What We Are Not, What We Are*

### 1.1 미션 (v1·v2 carry, 변경 없음)
*Mission Statement (Carried, Unchanged)*

한국의 정치 정보 환경은 구조적으로 극단화되어 있다. 지역 정서와 정당 충성도가 논리적 판단보다 우선하고, 같은 사실을 두고도 해석과 수용이 당파적으로 갈라진다. 이 구조 위에서 YouTube는 한국 정치 담론의 가장 큰 시장이 되었고, 그 안에서 왜곡된 주장·출처 불명의 루머·클릭베이트가 빠르게 확산되고 있다. 그러나 어떤 콘텐츠가 어디에서 시작되어 어떻게 퍼지는지 체계적으로 확인할 수 있는 공공 인프라는 존재하지 않는다. 2026년 6월 지방선거는 이 공백이 실제 투표 행동에 영향을 미치는 고위험 구간이다.

**PRISM 한 줄 정의 (v3 갱신):**
> 한국 정치 YouTube에서 클레임이 어디서 시작되어 어떻게 변형되며 퍼지는지를 매일 추적하고, 그 흐름과 출처를 사용자가 직접 확인할 수 있도록 보여주는 흐름 매체.

### 1.2 우리가 아닌 것 (5가지, v2 carry)
*What We Are Not (Five Negative Definitions)*

PRISM의 정체성을 보호하는 governance lock. 6명 팀이 흔들리지 않게 하기 위한 정의이며, 외부 압력에 대한 1차 응답 자료.

1. **사회·경제 zeitgeist 도구가 아니다.** 한국 사회 전반의 핫뉴스를 정리하는 일반 큐레이션 서비스가 아니다. 사회·경제 lived experience와 *연결되는* 정치 클레임만 다루며, 정치와 무관한 사회·경제 이슈 자체는 다루지 않는다.

2. **후보 평가 도구가 아니다.** "이 후보가 좋다 / 나쁘다" 판단을 사용자에게 제공하지 않는다. 후보 발언의 출처를 추적하고 관련 canonical source를 병렬 제시할 뿐, 후보 자체에 대한 정성적 평가는 하지 않는다.

3. **팩트체크 도구가 아니다 (v3 강화).** "이 발언은 거짓이다", "사실과 다름", "True/False/Misleading" 라벨링을 하지 않는다. 명예훼손법 + 공직선거법 250조 (후보자 비방) + 82조의4 (허위사실공표)는 모두 *판단·평가 진술*에 적용되며, PRISM은 그 적용 대상이 되지 않도록 사실 병렬 제시만 한다. SNU팩트체크·JTBC팩트체크·뉴스타파와 다른 layer에 위치한다 (구체적 차별화는 §6 참고). **이 명시는 사이트 진입 시 사용자 facing 노출 (이용약관 형식 + About 페이지) — 팀 내부 원칙이 아니라 사용자에게 공식 공개되는 약속.**

4. **투표 추천 도구가 아니다.** "어느 후보를 뽑으세요" 또는 그에 준하는 nudge를 일체 제공하지 않는다. 사용자의 투표 결정은 사용자의 영역이다. 선거기 한정 기능 (투표소 알림 등)도 추가하지 않는다 — 핵심 mission에서 멀어진다.

5. **분류 도구가 아니다 (Show, don't classify).** "왜곡됨", "오해 소지", "True", "Misleading" 등 어떠한 평가 라벨도 카드에 부여하지 않는다. 출처와 데이터를 병렬로 보여줄 뿐, 분류는 사용자의 인지 영역에 남긴다. (이 원칙은 §4.2에서 구체화.)

### 1.3 우리가 무엇인가 — 흐름 매체 (v3 신규)
*What We Are — A Flow Medium (New in v3)*

v2까지 PRISM의 self-positioning은 negative ("우리는 ~이 아니다") 중심이었다. 5/5 김현미 인계 미팅에서 positive self-positioning이 한 줄로 정리되었다.

> **PRISM은 정보가 어떻게 변형되며 퍼지는지를 매일 다루는 한국 유일의 흐름 매체다.**

**왜 이 framing이 중요한가:**

(a) **Misinformation의 실질 피해를 가장 명확히 보여줄 수 있는 layer.** 가짜 정보가 왜 문제인지 추상적으로 답하기 어렵지만, 선거에서는 명확하다. 선거는 우리 삶에 직접적 영향을 미치는 의사결정인데, 거기에 잘못된 정보가 퍼지면 직접 피해가 발생한다. 이번 지방선거 국면을 "정보 흐름의 가시화"로 활용한다.

(b) **훈계조 회피의 구조적 답.** "정치를 공부해야 한다" 또는 "이런 측면을 보셔야 한다"고 말하지 않고, 다음 세 가지를 보여주기만 한다:
   1. 사람들이 관심 가지는 정보가 구체적으로 어떻게 퍼지는지
   2. 지금 접하고 있는 정보가 어떻게 잘못되고 있는지
   3. 제대로 알아두면 좋을 정보는 무엇인지

(c) **흐름 누적이 곧 매체 정체성.** 같은 이슈가 반복적으로 다뤄지면 시간 축 흐름이 자연스럽게 축적된다. 매일 5장 + 토요일 Weekly Flow 1장 (§5.1) 구조 자체가 "흐름 매체"의 form factor다. PRISM을 사용한다는 것은 "오늘의 사실"이 아니라 "이 클레임이 어떻게 변형되며 일주일 동안 움직였나"를 따라가는 경험.

(d) **장기 vision: Knowledge Graph로의 진화.** 정보가 꾸준히 쌓이면 v1의 KG ontology 비전 (Claim·Source·Channel·Politician·Event 등 12 node types)이 자연스럽게 매체 자산으로 성장한다. 정치 관련 백과사전, 정치인 발언 archive, 정책 이슈 timeline 등으로 확장 가능 — Track 2 (2027 대선) 가치 제안의 기반.

**Implication for product 결정:**
- 카드 form factor (§4.1)에 "시간 순서" + "출처 성격" 메타 줄 필수 추가 — 흐름 매체의 user-facing 구현체.
- Cadence (§5.1)에 토요일 Weekly Flow 추가 — 흐름 누적의 명시적 단위.
- 차별화 표 (§6.1)에 "흐름 매체" 위치 명시.

이 5가지 부정 + 1가지 긍정이 PRD-v3의 governance lock + positioning lock 동시 수행.

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

**확장 가능 secondary target (이슈 framing으로 진입):**
- 정치에 사전 관심이 적은 20~30대 — 본인 삶과 직결된 이슈 (주거·노동·AI 규제 등)로 진입하면 카드 진입 가능. 이슈 라벨링 시스템 (§2.3)이 이 진입을 매개.

**언론·연구자 (3차)**: post-launch organic engagement 대상. 의도적으로 pre-launch 파트너십·outreach 없음.

### 2.2 콘텐츠 범위
*Content Scope*

**In Scope:**
- 한국 정치 YouTube 콘텐츠 (정치인·정당·정치 채널·정치 토론)
- 정치 클레임의 canonical source 추적 (§3)
- 이슈 단위 큐레이션 (§2.3)
- 클레임의 시간 축 변형 추적 (§4.1 시간 순서 메타)

**Out of Scope:**
- 사회·경제 zeitgeist 일반 (정치와 연결 안 되는 부분)
- TV 토론회 영상 분석, 외신, Substack·개인 블로그 (v1·v2 carry)
- ASR / 비디오 콘텐츠 자체 분석 (v1·v2 carry, Track 2로 이연)
- 카톡봇, 브라우저 확장 (post-MVP)
- 정치인 중심 drill-down 뷰 (이슈 framing이 우선)
- 자동 claim verification / 분류 라벨 (§4.2)
- 선거기 한정 기능 (투표소 알림 등) — §1.2 #4

### 2.3 이슈 분류 체계
*Issue Taxonomy*

PRISM은 **이슈 단위**로 사용자에게 진입한다. 후보·정당·이벤트 단위가 아니다. 정치 fatigue 사용자가 본인 삶과 직결된 이슈를 통해 정치 콘텐츠에 접근하도록 하기 위함이다.

**1차 이슈 카테고리 (잠정 8개, MECE 재검증 필요 — v3 신규 표시):**
1. **주거** — 청년 무주택, 부동산 정책, 임대차 등
2. **노동** — 최저임금, 노동시간, 청년 일자리, 산재 등
3. **교육** — 입시 정책, 사교육, 대학 등록금 등
4. **AI 규제** — AI 정책, 데이터 규제, 디지털 노동 등
5. **의료·복지** — 건강보험, 노인 복지, 출산 정책 등
6. **안보·외교** — 한미관계, 한일관계, 북한 등
7. **사법·민주주의** — 검찰 개혁, 언론 자유, 선거 제도 등
8. **지역** — 수도권 집중, 지역 균형 등 (지방선거 특성 반영)

**🔴 Open question (v3 신규, 5/10 토 PRD 확정 전 결정):**
이 8개가 정말 *사회 전반, 특히 사람들이 관심 가지는 사회 전반*을 MECE하게 잘 포함하는가? 김현미 피드백:
> "8개 카테고리에 대해, 이게 정말 사회 전반, 특히 사람들이 관심 가지는 사회 전반을 MECE하게 잘 포함한 것이고, 그 이후에는 잘 선택해둔 것이 맞는지 한 번 더 고민해보면 좋겠다."

**검증 방법 (5/6~5/9):**
- 한국 주요 여론조사 (한국갤럽·리얼미터 등) "현 정부 가장 시급한 과제" 응답 카테고리와 비교
- 2026 지방선거 후보자 공약집 카테고리 분포와 비교
- 10명 미만 친구·동료 sanity check (이 8개로 본인 삶 이슈가 다 잡히는가)
- 누락 가능성: 환경·기후, 젠더·인구, 경제 일반 (성장·인플레), 외국인·이민

이슈 카테고리는 launch 후 사용 데이터 기반으로 조정 가능. **카테고리 자체가 fixed PRD가 아님 — taxonomy는 큐레이션 워크플로우에서 진화.**

**카드의 진입 라벨은 이슈 단위:**
- 좋은 예: "청년 무주택 — 이번 주 정치권 인용 통계와 원본"
- 나쁜 예: "후보 X 노동 정책 발언 출처 추적"

같은 데이터, 다른 진입점. 이슈 framing이 비정치 사용자도 클릭할 수 있게 한다.

---

## 3. 데이터와 출처
*Data Sources & Canonical Source Graph*

### 3.1 Canonical Source 7종 (v1·v2 carry)
*Seven Canonical Sources*

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

### 3.2 Canonical Source Graph (v2 carry)
*Pre-Built Canonical Source Graph*

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

### 3.3 `data.go.kr` 공공데이터포털 (v3 신규, BIGKinds 대체)
*data.go.kr (New in v3, Replaces BIGKinds)*

v2의 BIGKinds 무료 웹 review는 v3에서 **삭제**되고 `data.go.kr` (공공데이터포털)로 대체된다. 5/5 [[김재훈]] 콜에서 결정:

**BIGKinds 거부 이유:**
- 학술 할인 적용해도 ~80만원/월 견적 → Track 1.5 단계에 비용 정당화 불가
- 본문 200자 제한 → 큐레이션 quality 부족
- 조선·동아 등 주요 매체 누락 (단가 거부)
- API "AI용(학습·파인튜닝·RAG) 활용 불가" 약관과 PRISM의 LLM 활용 architecture 충돌

**`data.go.kr` 활용 (무료 키):**
- **국회 속기록·회의록** — 정치인 발언의 1차 출처
- **정책브리핑** — 정부 정책 발표 archive
- **선거 정보지** — 선관위 공식 데이터 (후보자 등록 정보 포함)
- 추가 가능 dataset 탐색은 데이터 담당 (재훈) 합류 후 진행

**데이터 신뢰성 ranking (carry from v2):**
1. data.go.kr (정부 1차 출처) > 7종 canonical source > 주요 언론 보도 > 정치 YouTube 클레임

**활용 방식:**
- 큐레이터가 매일 아침 monitoring 시 data.go.kr 관련 dataset 자동 lookup → 카드의 canonical source 병렬 제시 라인에 배치
- 후보자 발언이 data.go.kr에 등록된 통계·정책 자료와 충돌 시, 양쪽을 평등하게 병렬 제시 (어느 쪽이 맞는지 판단 안 함, §4.2 원칙 준수)

**API 키 발급 신청:**
- `data.go.kr` 무료 키 발급 신청 필요 (선거 정보지·국회 속기록·정책브리핑 dataset 각각 별도 키 필요할 수 있음). 5/10 PRD 확정 전 신청 진행.

**보조 input signal (v3 carry from v2 spirit):**
- BIGKinds 무료 웹의 "오늘의 이슈" / "오늘의 키워드"는 *큐레이터 manual review용 보조 신호*로 비공식적 활용 가능 (저장·LLM 주입 없음). 단 PRISM 공식 데이터 소스에는 포함 안 함.

**Why 무료로 충분한가:** PRISM은 카드 5장/일 (+ 토요일 1장) produce. data.go.kr + YouTube API + canonical source 7종으로 매일 cover 가능. 데이터 quality 차이는 launch 후 실측 후 재평가.

### 3.4 정치 YouTube monitoring
*Political YouTube Monitoring*

v1의 F1 (YouTube 데이터 파이프라인)을 v3 운영에 맞게 재정의:

**Discovery:**
- 시드 채널 40-60개 (한국 정치 YouTube 주요 채널 — 좌·우 균형)
- YouTube 트렌딩 정치 카테고리 일일 review
- 후보자명 쿼리로 누락 보강

**수집 신호 (v1 22개 신호 carry):**
- API: 제목, 설명, 태그, duration, 조회수, 업로더, 업로드 시각, 채널 메타, Community tab, 댓글 (top + newest + pinned), topicCategories, 트렌딩 진입, 자막
- 파싱: Description URL·chapters
- AI 파생: 썸네일 OCR, 썸네일 패턴, 감성, 클릭베이트 탐지, 댓글 봇 탐지

**v3 운영 layer:**
- 일일 trending claims 자동 추출 (LLM assist) → 큐레이터 검토 큐
- Canonical Source Graph 자동 lookup → 큐레이터에게 매칭 후보 제시
- data.go.kr cross-reference → 통계·정책 충돌 시 자동 flag
- 큐레이터가 카드 5장 (+ 토요일 1장) 선정 + 작성 + approve

**기술 stack 결정 (담당자 위임):** YouTube Data API v3 + yt-dlp 백업, Python 큐레이션 백엔드. 김재훈이 개발 리드.

**크롤링 안 함:** 법적 risk + 안정성 부족. API + RSS + manual lookup만.

---

## 4. 코어 제품
*Core Product*

### 4.1 카드 form factor (v3 메타 두 줄 추가)
*Card Form Factor (Two New Meta Lines in v3)*

PRISM의 사용자 facing primary unit은 **카드**. 카드는 모바일 우선 디자인이며, 카톡·X 공유에 최적화된다.

**카드 구성요소 (필수 10개, v3 추가 2개):**

1. **이슈 라벨** (예: "주거", "노동", "AI 규제") — 진입 식별자
2. **카드 제목** — 이슈 + 한 줄 핵심 (예: "청년 무주택 — 이번 주 정치권 인용 통계와 원본")
3. **본문 (200-400자)** — 어떤 클레임이 어떻게 돌고 있는지 factual 기술
4. **클레임 병렬 제시** — 정치 YouTube에서 발견된 2-4개 클레임을 발화자·발화일·발언과 함께 나열
5. **시간 순서 메타 (v3 신규)** — 각 클레임에 "가장 먼저 등장 / 24시간 후 / 익일 변형 / 주말 동안 확산" 등 시간 축 라벨. 흐름 매체 정체성 (§1.3)의 user-facing 구현체.
6. **출처 성격 메타 (v3 신규)** — 각 클레임에 "정치인 직접 발언 / 언론 인용 / 익명 커뮤니티 유출 / 2차 가공 채널" 등 source-type 라벨. 사용자가 정보 출처의 위계를 한눈에 인지.
7. **Canonical source 병렬 제시** — 관련 통계청·부처·후보 공식 발표·data.go.kr dataset 등 1-3개 출처와 함께 나열
8. **출처 링크 (전부)** — 각 클레임의 영상 링크 + 각 canonical source의 원본 링크
9. **Provenance footer** — "PRISM은 발언을 분류하지 않습니다. 출처를 보여드립니다. 판단은 사용자 본인의 몫입니다." + "사람이 검수했습니다."
10. **공유 버튼** — 카톡, X, 링크 복사

**카드 form factor 예시 (v3, 메타 줄 포함):**

```
[이슈 라벨: 주거]

청년 무주택 — 이번 주 정치권 인용 통계와 원본

이번 주 정치 YouTube에서 청년 무주택 통계 인용이
다음 세 가지 형태로 확산되었습니다.

[클레임 — 시간 순서 + 출처 성격]
1. 후보 X (4/27 영상)
   • 발언: "청년 50% 무주택"
   • 시간: 가장 먼저 등장
   • 출처 성격: 정치인 직접 발언

2. 후보 Y (4/28 토론회)
   • 발언: "청년 38% 무주택"
   • 시간: 24시간 후 (X 발언에 반박)
   • 출처 성격: 정치인 직접 발언

3. 정치 채널 Z (4/29 영상)
   • 발언: "청년 무주택 60% 시대"
   • 시간: 익일 변형 (X 수치 확대)
   • 출처 성격: 2차 가공 채널 (X 영상 인용)

[관련 canonical source]
- 통계청 2025 인구주택총조사: 청년 35% 무주택
- 국토부 2025 보고서: 19~34세 무주택 38%
- data.go.kr 청년주거실태 dataset (2025-12 갱신)

[출처]
- 후보 X 영상 [링크]
- 후보 Y 발언 [링크]
- 정치 채널 Z 영상 [링크]
- 통계청 원본 [통계청.kr]
- 국토부 보고서 [molit.go.kr]
- data.go.kr [링크]

PRISM은 발언을 분류하지 않습니다.
출처를 보여드립니다.
판단은 사용자 본인의 몫입니다.
사람이 검수했습니다.

[카톡 공유] [X 공유] [링크 복사]
```

**카드 톤 가이드라인:**
- 표준 존댓말 ("~합니다", "~입니다")
- Factual + 약간의 인간미 (캐릭터 voice X, 위트 X, 풍자 X)
- 정치 평가 표현 금지 ("우려된다", "주목된다", "심각하다" 등)
- 형용사·부사 최소화 (객관성 보호)

### 4.2 "Show, don't classify" 원칙 (v2 carry, v3 변경 없음)
*The "Show, Don't Classify" Principle*

PRD-v2의 가장 중요한 product 결정. PRISM은 어떠한 평가 라벨도 부여하지 않는다.

**금지되는 표현:**
- ❌ "왜곡됨", "거짓", "오해 소지", "맥락 부족"
- ❌ "True", "False", "Misleading", "Mostly True"
- ❌ "사실과 다름", "검증됨"
- ❌ "주의 필요", "확인 요망"

**허용되는 표현 (v3 추가):**
- ✅ "다음과 같은 통계가 있습니다"
- ✅ "통계청 자료에 따르면"
- ✅ "원본을 확인할 수 있는 자료는 다음과 같습니다"
- ✅ "관련 정부 발표는 다음과 같습니다"
- ✅ "오마이뉴스 4월 28일 기사에 따르면 발언이 있었다고 합니다" — *발언 진위*가 아니라 *보도 사실*을 인용하는 형태 (5/5 재훈 콜)

**이 원칙이 풀어주는 3가지:**
1. **법적 risk 최소화** — 명예훼손·허위사실공표 노출 거의 0
2. **사용자 자율성 존중** — 가르치는 톤 0
3. **운영 부담 감소** — 판정 부담 사라짐, daily intensive cadence 가능

**원칙 위반 감지 mechanism:**
- 모든 카드는 publish 전 human approver review (§5.2)
- Approver checklist에 "분류 라벨 포함 여부" 항목 명시
- 위반 시 publish 거부, 큐레이터에게 reword 요청

### 4.3 Provenance UI surface
*Provenance UI Surface*

v1 §F4 KG ontology의 사용자 facing UI 형태.

**카드 안 provenance 시각화:**
- 각 클레임에 "발화자 / 발화일 / 영상 위치 / 시간 순서 / 출처 성격" 메타데이터 visible
- 각 canonical source에 "출처 기관 / 발표일 / 원본 URL" visible
- 클레임과 canonical source가 *충돌*할 때, 양쪽을 평등하게 병렬 제시 (어느 쪽이 맞는지 판단 안 함)

**확장 view (post-launch 검토):**
- 클레임 graph view (이 클레임이 어디서 어디로 퍼졌나)
- 시간 축 view (이 이슈가 어떻게 진화했나)
- 채널 cluster view (어느 진영의 어떤 채널이 다뤘나)

**Track 1.5 launch는 카드 view + 토요일 Weekly Flow view.** 그래프·시간축·클러스터 view는 post-launch 검토.

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
- 토요일 오전 9시 Weekly Flow 1장 드롭 (주말 카톡 공유 peak)

**Why 카톡 우선:** 한국 사용자의 정치 콘텐츠 공유는 X (5%) < Facebook (10%) < 카톡 (60%). 카톡 미리보기 미최적화 = viral 자살.

### 4.5 Process Transparency Disclosure (v3 신규)
*Process Transparency Disclosure (New in v3)*

PRD-v2의 "PRISM은 LLM 단독 판단 안 합니다" surface (§6.2)를 한 단계 더 끌어올려, **인간이 어디에서 어떻게 검수·관리하는지 그 절차 자체를 사용자가 볼 수 있는 페이지로 공개한다.** 5/5 재훈 콜 결정.

**Dual purpose:**
1. **Implicit signaling of objectivity intent** — "우리는 객관적으로 일하려고 유념하고 있다"는 것을 노골적 주장이 아니라 *절차의 가시화로* 전달. 사용자가 "이 사람들이 룰대로 하려고 노력은 하는구나"를 자력으로 추론 가능.
2. **Preemptive self-protection (legal alibi)** — 향후 명예훼손·편향 시비가 생겼을 때, "강동재 개인이 임의로 한 게 아니라 사전에 공개된 룰대로 했다"는 alibi 확보. 사후 변명이 아니라 *사전 표명*이라는 점이 핵심.

**5가지 결정 (v3 lock):**

| 항목 | 결정 |
|---|---|
| **(a) 위치** | 별도 "How We Work" 페이지 (사이트 navigation에 명시), About 페이지에서 hyperlink, 카드 footer "사람이 검수했습니다" 링크. |
| **(b) 상세도** | §5.1 daily workflow + §5.2 approver checklist 6개 항목 그대로 노출. 큐레이션 시간대·역할 분담·gate 작동 방식까지 포함. |
| **(c) 큐레이터 신원** | "edit team"으로 *역할 단위* 노출 (Lead Curator / Approver Pool / Backend / Design). 개인 실명은 Jack 1명만 노출 (책임자 명시), 나머지는 익명. 사후 추가 공개는 본인 동의 시 가능. |
| **(d) 가이드라인 changelog** | 공개. PRD 자체를 GitHub public repo에 공개 (이미 진행 중)하고, "How We Work" 페이지에 PRD 변경 history 요약 링크. 사후 큐레이션 가이드라인 변경도 changelog로 기록. |
| **(e) 진입 modal vs footer** | **Footer 링크만**. 진입 시 modal 강제는 사용 마찰 + 사용자 경험 손상. About 페이지 + footer 링크로 충분 — *접근 가능성*이 핵심이지 *강제 노출*이 핵심이 아님. 단, 카톡 공유로 진입한 1회성 사용자도 footer 링크는 visible. |

**페이지 구조 (drafts):**
```
"How We Work" 페이지 구조

1. 우리는 무엇을 하지 않는가 (§1.2 5가지 negative definition)
2. 우리는 무엇인가 (§1.3 흐름 매체 self-positioning)
3. 매일의 작업 흐름 (§5.1 daily workflow 그대로)
4. 검수 체크리스트 (§5.2 approver checklist 6개)
5. 우리가 신뢰하는 출처 (§3.1 7종 canonical + data.go.kr)
6. 우리가 사용하는 AI (§5.3 AI 활용 layer + 자동화 안 하는 항목)
7. 책임자 (Jack 실명 + 역할 단위 팀 구성)
8. PRD 변경 history (GitHub public repo 링크)
9. 정정·제보 channel (이메일 또는 form)
```

**왜 이 페이지가 PRD 본문 §1.3 (흐름 매체)와 묶이는가:**
흐름 매체는 정의상 "어떻게 움직였나"를 보여주는 매체. 그런데 PRISM 자체가 어떻게 움직이는지 (큐레이션 흐름) 안 보여주면 self-contradiction. Process Transparency Disclosure는 PRISM이 자기 자신에게 흐름 매체 원칙을 적용한 것.

---

## 5. 운영
*Operations*

### 5.1 Daily curation workflow + Weekly Flow (v3 토요일 추가)
*Daily Curation Workflow + Saturday Weekly Flow*

**Cadence (v3 갱신):**
- **월~금**: 매일 카드 5장 publish
- **토**: **Weekly Flow 카드 1장** publish — 그 주 가장 활발히 변형된 클레임 1개를 깊게 추적. 평일 카드를 raw material로 재활용 → 신규 production 부담 최소화.
- **일**: 휴식 (선택적 light cadence 1-2장 OK)
- **D-7 ~ D-1 (5/27 ~ 6/2)**: 매일 카드 7-10장 escalation
- **D-Day (6/3)**: 별도 운영 (실시간 monitoring + 사후 분석)
- **D+1 이후**: 일일 카드 3장 정리 + Weekly Flow 토요일 carry

**Weekly Flow의 형태 (v3 신규):**
- 한 주 카드 25-30장 중 가장 활발히 변형된 클레임 1개 선정
- 시간 축 timeline 명시 (월요일 등장 → 화요일 변형 → 수요일 확산 → 목요일 카운터 → 금요일 진정)
- 클레임 변형 단계마다 영상 link + canonical source 비교
- "이번 주의 흐름" 헤더로 PRISM의 흐름 매체 정체성 직접 surface
- 토요일 오전 9시 드롭 (주말 카톡 공유 peak 활용)

**일일 워크플로우:**

```
오전 8:00  큐레이터 lead (Jack) data.go.kr + YouTube monitoring 결과 review
오전 8:30  AI assist로 trending claims 자동 추출 → 큐레이터 검토 큐
오전 9:00  큐레이터 lead가 그날 카드 후보 10-15개 선정
오전 9:30  AI assist로 카드 초안 자동 작성 (claim + canonical source lookup + 시간/출처 메타)
오전 10:00 큐레이터 lead가 5장 final 선정 + 인간미·톤 가다듬기
오전 11:00 다른 팀원 1명이 publish gate review (§5.2)
정오 12:00 5장 publish + 카톡 공유 trigger
오후       사용자 반응·공유 monitoring + 다음날 준비
```

**토요일 워크플로우:**
```
오전 7:00  Lead 큐레이터가 한 주 카드 25-30장 review
오전 8:00  가장 활발히 변형된 클레임 1개 선정
오전 8:30  Weekly Flow 카드 작성 (시간축 + 클레임 변형 timeline)
오전 9:00  Approver review + publish
```

### 5.2 1 human approver gate (publish lock, v2 carry)
*Single Human Approver Gate*

PRD-v2의 second load-bearing pillar. v1의 "≥95% recursive gate"의 운영 가능 형태.

**Gate 정의:**
> PRISM card는 최소 1명의 human approver의 명시적 approval 없이 publish 되지 않는다.

**Approver checklist (publish 전 review 항목, 6개):**
1. 클레임 원본 영상 link 작동 여부 + 발언 정확성
2. Canonical source 원본 link 작동 여부 + 인용 정확성 (data.go.kr dataset 포함)
3. 분류 라벨 포함 여부 (§4.2 위반 감지)
4. 정치 평가 표현 포함 여부 (§4.1 톤 가이드라인 위반 감지)
5. 후보 평가·투표 추천 함의 포함 여부
6. 명예훼손 위험 평가 (특정인을 부정적으로 묘사하는 표현 없는지)

체크리스트 6개 모두 통과 + approver 명시적 approval → publish.
하나라도 미통과 → 큐레이터에게 reword 요청, re-review.

**Approver rotation (v3 — 6명 팀 기준):**
- 6명 팀에서 rotation. Lead 큐레이터 (Jack)가 작성한 카드는 Jack 외 1명이 review.
- Approver 본인이 작성한 카드는 본인이 review 못 함 (self-approval 금지).
- Approver 이름은 카드 metadata에 기록 (사후 추적 가능, 사용자 facing 안 함).

**Gate 실패 시 escalation:**
- 큐레이터-approver 사이 의견 충돌 → 팀 group chat으로 escalation
- 합의 못 하면 publish 보류 (그날 카드 4장 또는 3장으로)
- "publish 못 한 카드"가 누적되면 weekly review에서 패턴 분석

### 5.3 AI 활용 layer (v2 carry)
*AI Utilization Layer*

PRISM은 AI를 적극 활용한다. 단, **publish 결정은 안 한다.**

**AI 자동화 항목:**
- data.go.kr dataset 자동 lookup
- YouTube trending claims 자동 추출
- Canonical Source Graph 자동 lookup
- 카드 초안 자동 작성 (claim + source 병렬 텍스트 생성)
- 시간 순서 / 출처 성격 메타 자동 분류 (큐레이터 검수)
- 톤·스타일 일관성 check
- 분류 라벨 포함 여부 자동 감지 (§4.2 위반 sentinel)
- 카톡 미리보기 메타데이터 자동 생성

**AI 자동화 안 하는 항목 (사람이 함):**
- 그날 publish할 5장 (+ 토요일 1장) final 선정
- 카드 톤·인간미 최종 가다듬기
- 시간 순서 / 출처 성격 메타 최종 검수
- Publish gate approval (§5.2)
- Risk 판정 (명예훼손·정치 평가)

**AI model 선택 (담당자 위임):**
- Claim 추출·요약: GPT-4 / Claude / Gemini 등 (cost·quality 비교)
- Embedding (claim clustering): KoSimCSE 또는 다국어
- Korean NER: BERT 한국어 fine-tuned (v1 그대로)

**AI 비용 budget:**
- Track 1.5 launch 기간 (5/4 ~ 6/3 = 30일): 월 ~$200-500 USD 예상
- 부담 source 확정 필요 (Jack 본인 부담 vs SIPA stipend vs 팀 분담)

### 5.4 6명 역할 분담 (v3 갱신 — 풀타임 표기 폐기)
*Six-Person Team Allocation (No Full-Time Assumption)*

**운영 전제 (v3 갱신):**
6명 모두 학업·직장·다른 프로젝트를 병행하며, 각자 가용한 시간을 적극 투입한다. 토요일 작업은 정상이며, 일정은 마일스톤 단위로만 의미가 있다.

| 역할 | 담당 | 일별 commitment |
|---|---|---|
| Lead + 큐레이션 큐레이터 | Jack | 매일 ~3시간 (큐레이션 lead, 변동 가능) |
| Backend / pipeline 리드 | 김재훈 | 가용 시간 적극 투입, 개발 리딩 + 합류 멤버 협업 방식 결정 권한 위임 |
| Governance / 거버넌스 + 이슈 큐레이션 보조 | 정아인 | 가용 시간 적극 투입 (approver rotation 포함) |
| 디자인 / UX 리드 | 김현미 | 가용 시간 적극 투입 (이미 Figma 등 진행 중) |
| Provenance UI / 디자인 | Saetbyeol Leeyouk (MIT Media Lab) | **5/11(월) 본격 합류** 후 가용 시간 |
| Frontend / 개발 | 샘 리 (Sam Lee, EPFL Computer Science) | **카톡방 합류 완료, 5/11~ 본격 시작.** 하루 2-3시간 가용 |

**Jack daily commitment (PRD에 명시):**
> Jack은 launch 기간 (5/4 ~ 6/3) 매일 lead 큐레이션을 직접 담당하며, daily cadence + 카드 quality + 분류 라벨 위반 감지에 직접 책임을 진다. 본인 학업 일정과 충돌 시 사전 공지 + 백업 큐레이터 (정아인) 지정.

**Approver rotation schedule (예시 — 6명 가용 패턴 따라 조정):**
- 평일 매일 1명 approver 배정 (rotation)
- 토요일 Weekly Flow는 Lead (Jack) 외 누구든 1명
- Approver 본인 가용성 우선, 충돌 시 group chat으로 swap

**개발 리딩 (5/5 재훈 콜 결정):**
> 김재훈이 개발 리드. Saetbyeol (5/11 합류) + 샘 리 (5/11~ 본격 시작)와의 협업 방식 자율 결정. 재훈이 편한 협업 방식으로, 개발을 리드하면서 두 명을 활용해 같이 진행.

**일정 원칙:**
- AI 자동 생성 일정 (week-by-week 표)에 매이지 않음
- 마일스톤만 명시: ① 5/10 (토) PRD 확정 + 데모 빌드 + 디자인 1차 점검, ② 5/11 (월) 6명 team kick-off (Saetbyeol + Sam Lee 합류 후 첫 정기 미팅), ③ 5/30 (토) public launch, ④ 6/3 (수) 선거일.
- Week 2 (5/11~) 부터 6명 다 함께 정기 미팅으로 일정 결정 — AI 자동 생성 일정 폐기.

**정기 미팅 슬롯 (5/5 재훈 콜):**
- KST 평일 저녁 21:00–24:00 (= NYC 오전 8:00–11:00)
- 5/11 (월) 합류 후 6명 슬롯 lock

---

## 6. 차별화와 포지셔닝
*Differentiation & Positioning*

### 6.1 경쟁 도구 대비 차별화 (v3 — 흐름 매체 위치 명시)
*Competitive Differentiation (v3 — Flow Medium Positioning)*

| 도구 | 강점 | PRISM 대비 약점 |
|---|---|---|
| **ChatGPT / Perplexity / Claude** | Universal Q&A, 빠름, 무료/저렴 | Pre-built Korean political graph 없음. Real-time YouTube provenance 못 함. Hallucination risk. |
| **뉴닉** | 정리 잘된 daily news, 가벼운 캐릭터 voice | 사회 일반 (정치 미스인포 specific X). 클레임 단위 추적 안 함. 시간 축 흐름 안 보여줌. 출처 병렬 안 함. |
| **슬로우뉴스** | 깊은 시사 분석, 신뢰 톤 | Weekly+ cadence (daily 아님). 시간 축 흐름 안 보여줌. 클레임 단위 아님. |
| **피렌체의 식탁** | 큐레이션 quality | 사회 일반. 정치 클레임 specific 안 함. |
| **SNU팩트체크 / JTBC팩트체크 / 뉴스타파** | 깊은 fact-check, 신뢰성 | 라벨링 (True/False) 적용 — PRISM이 의도적으로 회피하는 layer. 사후 + 느림. YouTube ecosystem 분석 안 함. 흐름 안 보여줌. |
| **Snopes / Ground News (영문)** | 글로벌 모델 | 한국어 정치 이해 없음. Korean canonical source 없음. |

**PRISM의 unique 위치 (v3 한 줄):**
> **PRISM은 한국 정치 정보가 어떻게 변형되며 퍼졌나를 매일 추적하는 흐름 매체다.** Provenance를 분류 없이, 시간 순서 + 출처 성격 메타와 함께, canonical source graph + data.go.kr 기반으로 제공한다. 한국에 동일 layer의 매체 부재 — white space.

**왜 "흐름 매체"가 차별화의 핵심인가:**
- 뉴닉/슬로우뉴스: 정보 정리 (what)
- 팩트체크 도구: 사실 판정 (true/false)
- ChatGPT: Q&A (how)
- **PRISM: 정보가 어떻게 움직였나 (how it moved)** ⭕

이 layer는 한국에 prior art가 없음. Track 1.5 launch가 white space의 첫 점유.

### 6.2 "PRISM은 LLM 단독 판단 안 합니다" surface (v2 carry)
*Ethical Lock as User-Facing Message*

PRD-v2의 marketing/positioning 한 줄. 모든 사용자 facing 페이지에 명시. v3에서는 §4.5 Process Transparency Disclosure 페이지로 한 단계 확장.

**Surface 위치:**
- 사이트 footer
- About 페이지
- "How We Work" 페이지 (v3 신규, §4.5)
- 각 카드 footer (간략 버전: "사람이 검수했습니다")
- 카톡 공유 미리보기 description

**구체 문구 (한국어):**
> PRISM은 모든 카드를 사람이 직접 검수한 후 공개합니다. AI는 카드 작성을 보조하지만, 어떤 카드가 공개될지는 사람이 결정합니다. 우리는 출처를 보여드릴 뿐, 발언을 분류하지 않습니다. 우리가 어떻게 일하는지 절차도 공개합니다.

이 한 단락이 PRISM의 정체성 lock + 차별화 lock + 신뢰 lock 동시 수행.

### 6.3 한국어 정치 YouTube provenance white space (v3 — 흐름 매체 강조)
*Korean Political YouTube Provenance White Space (v3 — Flow Medium Emphasis)*

한국에 prior art가 거의 없음. 가장 가까운 시도들:
- **SNU팩트체크**: 라벨링 + 사후 + 사이트 시각화 약함, 흐름 안 보여줌
- **JTBC팩트체크**: 방송 형식 + scale 작음
- **뉴스타파**: 탐사보도 형식 + 일회성
- **뉴닉/슬로우뉴스**: 사회 일반 + 흐름 매체 아님

**PRISM이 채우는 white space:**
- Daily cadence (다른 도구는 weekly 또는 event-based)
- **흐름 매체 정체성 (시간 축 + 출처 성격 메타로 user-facing 구현)** ← v3 추가
- 클레임 단위 그래프 (다른 도구는 article 단위)
- 라벨 없음 (다른 도구는 라벨링 중심)
- YouTube ecosystem 직접 분석 (다른 도구는 텍스트 기사 중심)
- Canonical source graph 사전 매핑 (어떤 도구도 안 함)
- Process Transparency Disclosure 공개 (어떤 도구도 안 함) ← v3 추가
- Weekly Flow 1장 (그 주 흐름 통째로 추적, 흐름 매체의 시그니처 form factor) ← v3 추가

이 white space가 Track 2 (2027 대선)의 더 큰 가치 제안의 sandbox.

---

## 7. Sprint 계획 (v3 — 마일스톤 only)
*Sprint Plan (v3 — Milestones Only)*

v2의 week-by-week 풀타임 가정 일정은 폐기 (§5.4). 대신 마일스톤만 명시.

### 7.1 마일스톤 (v3 lock)
*Milestones (v3 Lock)*

| 시점 | 마일스톤 |
|---|---|
| **2026-05-10 (토)** | PRD v3 확정 + Claude Code 데모 빌드 완료 + 김현미와 디자인 디테일 1차 점검 (lo-fi 단계 단축) |
| **2026-05-11 (월)** | Saetbyeol Leeyouk + Sam Lee 본격 합류. 6명 team kick-off 정기 미팅 슬롯 lock |
| **2026-05-17 (일)** | Backend MVP demo (재훈 리드) + 카드 form factor 사이트 통합 + Process Transparency Disclosure 페이지 초안 |
| **2026-05-24 (일)** | Soft launch 준비 (beta 사용자 5-10명 invite) + 한국 변호사 preliminary check |
| **2026-05-30 (토)** | **Public launch.** 카톡·X 공유 push. Daily 5장 cadence 시작 |
| **2026-06-01 (월) ~ 6/2 (화)** | Election week escalation (daily 7-10장) |
| **2026-06-03 (수)** | **선거일.** 실시간 monitoring + 사후 정리 |
| **2026-06-04 (목) ~ 6/10 (수)** | Mandatory rest week + retro |

### 7.2 Launch readiness checklist
*Launch Readiness Checklist*

Public launch (5/30) 전 필수 통과 항목:

- [ ] PRD v3 확정 (5/10)
- [ ] Daily 5장 cadence 1주 연속 안정 운영
- [ ] Weekly Flow 토요일 1장 첫 발행 성공 (5/24 또는 5/31)
- [ ] Approver gate 모든 카드 적용 + checklist 6개 통과율 100%
- [ ] 분류 라벨 위반 0건 (지난 1주)
- [ ] 카톡 미리보기 정상 작동 (5개 device 테스트)
- [ ] Canonical Source Graph 후보자 매핑 ≥80% 완성
- [ ] data.go.kr API 키 발급 완료 + 자동 lookup 작동
- [ ] About 페이지 + "How We Work" 페이지 (Process Transparency Disclosure) 노출
- [ ] 한국 변호사 preliminary check 완료
- [ ] 8개 이슈 카테고리 MECE 재검증 완료 (§2.3)
- [ ] 6명 팀 burn-out 신호 monitoring (weekly retro)

### 7.3 Lo-fi 단계 단축 (v3 신규)
*Compressed Lo-fi (New in v3)*

5/5 재훈 콜 결정. 별도 lo-fi sketch 단계는 **건너뛴다**.

**워크플로우:**
1. Jack이 Claude Code로 데모 빌드 (5/6~5/9)
2. Jack + 김현미가 데모를 직접 들어가 디테일 정리 (스크린샷 기반)
3. 그 디테일이 정리된 데모 = lo-fi 간주
4. 디테일 다듬어서 hi-fi 진입

**Why:** 데모만 있으면 구석에 숨은 화면을 디자이너가 못 찾음. Jack + 현미가 함께 데모를 들어가 스크린샷 정리하는 것이 lo-fi의 핵심 작업. 별도 와이어프레임 시간 절약.

---

## Appendix A. 결정 reasoning trail
*Decision Reasoning Trail*

각 핵심 결정의 *왜*. Future Jack 또는 신규 합류자가 "왜 이렇게 했는가"를 추적할 수 있게 하는 reference.

### A.1 왜 daily intensive (weekly 아님) — v2 carry
2026-05-04 세션. 초기에는 weekly drop 제안되었으나 Jack이 "사용자에게 큰 매력이 없을 것 같다, 매일 업데이트가 필요하다"고 push back. Daily가 retention + 카톡 공유 trigger + 선거 시즌 attention curve에 맞음.

### A.2 왜 토요일 Weekly Flow 추가 (v3 신규)
2026-05-05 김현미 인계 미팅. PRISM의 self-positioning을 "흐름 매체"로 정의한 후, 흐름 매체 정체성을 가장 직접적으로 표현하는 form factor가 무엇인가의 답으로 도출. 평일 5장은 raw material로 자연 누적되므로 토요일 1장 합성에 신규 production 부담 최소.

### A.3 왜 Show, don't classify (라벨링 아님) — v2 carry, v3 강화
v2 §A.2 그대로. v3에서는 5/5 재훈 콜에서 "팩트체크 도구가 아니다" PRD §1.2 #3에 명시 강화 + 사용자 facing 공개 (이용약관 형식).

### A.4 왜 Process Transparency Disclosure 페이지 (v3 신규)
2026-05-05 재훈 콜. 사람 주관 개입 위험 (카드 정렬·문구 선택에 큐레이터 사상 들어갈 수 있음)에 대한 답으로 도출. 단순한 "가이드라인 있어요" 수준이 아니라 *절차 자체를 가시화*. Dual purpose: (1) 객관성 추구 의도 implicit signaling (2) 사전 자기 보호 alibi. 자세히는 §4.5.

### A.5 왜 BIGKinds 폐기 + data.go.kr (v3 변경)
2026-05-05 재훈 콜. BIGKinds 견적 검토 결과: (a) ~80만원/월 비용 정당화 불가 (b) 본문 200자 제한 (c) 조선·동아 누락 (d) AI 활용 약관과 PRISM architecture 충돌. data.go.kr이 무료 + 국회 속기록·정책브리핑·선거 정보지 포함으로 PRISM 신뢰성 layer에 더 적합.

### A.6 왜 1 human approver gate (≥95% 합의 아님) — v2 carry
v2 §A.3 그대로.

### A.7 왜 정치 YouTube only (사회/경제 zeitgeist 아님) — v2 carry
v2 §A.4 그대로.

### A.8 왜 이슈 framing (후보 framing 아님) — v2 carry, v3 MECE 재검증 추가
v2 §A.5 그대로. v3에서는 김현미 피드백 반영하여 "8개 카테고리가 정말 MECE인가" 재검증을 §2.3 open question으로 명시.

### A.9 왜 풀타임 가정 일정 폐기 (v3 변경)
2026-05-05 재훈 콜. v2의 week-by-week 풀타임 일정은 AI가 자동 생성한 것이며 6명 모두 학업·직장·다른 프로젝트를 병행하는 reality와 맞지 않음. 토요일 작업은 정상이고 일정은 마일스톤 단위로만 의미가 있다.

### A.10 왜 흐름 매체 self-positioning (v3 신규)
2026-05-05 김현미 인계 미팅. v2까지 PRISM의 self-positioning은 negative ("우리는 ~이 아니다") 중심이었고, positive 정의가 missing. 김현미가 "흐름 매체 (how it moved)"로 정리. 한국에 prior art 없음 = white space의 핵심 axis. 미스인포메이션의 실질 피해가 가장 명확하게 드러나는 layer (선거 의사결정).

### A.11 왜 Sam Lee (EPFL) 합류 (v3 신규)
2026-05-05 재훈 콜. 재훈이 EPFL 컴퓨터 사회사 전공 + 프론트엔드 가능 + 유튜브 영상 분석 경험 있는 샘 리를 소개. 적극적 의지 보임 (재훈 평가). 카톡방 합류 완료, 5/11~ 본격 시작. 6번째 멤버 확정으로 v2의 "6번째 TBD" 해소.

### A.12 왜 Web only (앱 X) — v3 신규 명시
2026-05-05 재훈 콜. (a) 앱스토어/플레이스토어 심사 시간이 launch timeline (5/30)에 위험 (b) 설치 마찰 회피 (c) 반응형이면 모바일 접근 가능. 카톡 공유 mechanic도 web URL이 가장 매끄럽다.

---

## Appendix B. 거부된 대안
*Rejected Alternatives*

PRD-v3 작성 과정에서 진지하게 검토되었으나 거부된 대안들. 향후 외부 압력으로 재제안될 가능성이 있어 명시적으로 기록.

### B.1 사회/경제 zeitgeist 통합 모델 (v2 carry)
거부. v2 Appendix B.1 reference. §1.2 #1 lock.

### B.2 분류 라벨 (True/False/Misleading) (v2 carry, v3 강화)
거부. v2 Appendix B.2 reference. v3 §1.2 #3 + §4.2 lock + 사용자 facing 공개 (이용약관 형식).

### B.3 Time-based UI escalation (v2 carry)
거부. v2 Appendix B.3 reference.

### B.4 BIGKinds API 구매 (v3 — Phase 2도 deferred)
v2에서는 Phase 2 검토 항목이었으나 v3에서는 5/5 재훈 콜 검토 결과 비용 + 약관 + 본문 제한 등 구조적 한계로 Track 2 (2027 대선) 시점까지 deferred.

### B.5 50개 정치 채널 인지 부담 입력 (v2 carry)
거부. v2 Appendix B.5 reference.

### B.6 캐릭터 voice / 위트 / 풍자 톤 (v2 carry)
거부. v2 Appendix B.6 reference.

### B.7 Native app 빌드 (v3 신규 거부)
거부. §A.12 reference.

### B.8 EPFL UI/UX 친구 추가 영입 (v3 신규 거부)
재훈이 샘 리 외 EPFL UI/UX 친구 추가 영입을 제안. Jack 거부 이유: (a) 김현미가 디자인 작업 가능 (b) 7명 팀의 협업 복잡도 증가 (c) 6명 lock으로 운영 안정. 필요 시 재논의.

### B.9 진입 시 modal 강제 (Process Transparency Disclosure 노출 방식, v3 신규 거부)
§4.5 (e) reference. Footer 링크로만. 강제 modal은 사용 마찰.

### B.10 큐레이터 실명 전체 공개 (v3 신규 거부)
§4.5 (c) reference. Jack 1명만 책임자로 실명 노출, 나머지는 역할 단위 익명. 본인 동의 시 사후 추가 공개 가능.

---

## Appendix C. Risks
*Risks*

### C.1 명예훼손 / 공직선거법 risk (v2 carry)
- **Mitigation**: §4.2 분류 라벨 금지 + §5.2 1 human approver gate + §4.5 Process Transparency Disclosure (legal alibi) + Week 3 한국 변호사 preliminary check + 24시간 내 retraction protocol.

### C.2 운영 burn-out risk (v2 carry, v3 갱신)
- v2의 "6명 모두 풀타임" 가정 폐기로 burn-out risk 자체는 줄어듬 (가용 시간 적극 투입 모델). 단 토요일 Weekly Flow 추가로 lead 큐레이터 부담은 약간 증가.
- **Mitigation**: Approver rotation + 일요일 휴식 + Weekly retro에서 burn-out 신호 monitoring + Election week 후 mandatory rest week.

### C.3 Scope creep risk (v2 carry)
- **Mitigation**: §1.2 5가지 + §1.3 흐름 매체 self-positioning + Appendix B 거부 alternatives 기록.

### C.4 차별화 erosion risk (v2 carry, v3 강화)
- v3에서 "흐름 매체" 위치 + Process Transparency Disclosure 추가로 차별화 axis 다층화. 하나의 차별화 element가 약화되어도 전체 positioning은 유지 가능.
- **Mitigation**: §6.1 + §6.2 + §6.3 + 본 PRD revision은 6명 팀 합의 + 명시적 PRD update 없이 변경 금지.

### C.5 한국어 엔티티·사실 추출 quality risk (v2 carry)
- **Mitigation**: KR-WordNet + 선관위·국회 DB grounding + Canonical Source Graph entity verification + Approver checklist #1, #2 + data.go.kr 1차 출처 cross-reference.

### C.6 YouTube API quota / 차단 risk (v2 carry)
- **Mitigation**: 시드 채널 limit 40-60 + yt-dlp 백업 + quota 상향 신청.

### C.7 8개 이슈 카테고리 MECE 부족 risk (v3 신규)
- **Risk**: 김현미 피드백 — 8개 카테고리가 사회 전반을 정말 MECE하게 cover하지 못할 가능성. 누락 가능성: 환경·기후, 젠더·인구, 경제 일반, 외국인·이민.
- **Mitigation**: §2.3 검증 방법 5/6~5/9 진행 (여론조사 cross-reference + 후보자 공약집 + sanity check). PRD 확정 (5/10) 전 카테고리 lock.

### C.8 Process Transparency Disclosure이 역효과 risk (v3 신규)
- **Risk**: 인간 개입 절차 공개가 사용자에게 "AI 자동화 못 한다"는 신호로 읽혀 ChatGPT 대비 약점으로 인식될 가능성. 또는 절차 공개가 "어차피 사람이 한다"는 회의로 이어질 가능성.
- **Mitigation**: 페이지 framing을 "신뢰의 근거"로 명시 + ChatGPT의 hallucination risk와 대비 + "사람이 검수한 출처 vs AI hallucination" 차별화 강조.

### C.9 6명 팀 협업 마찰 risk (v3 신규)
- **Risk**: 5/11 동시 합류 (Saetbyeol + Sam Lee) 후 6명 협업 형식 결정에 시간 소요. 개발 리딩이 재훈에게 위임되었지만 협업 방식 자체가 처음.
- **Mitigation**: 5/11 kick-off 미팅에서 협업 방식 명시 + 정기 미팅 슬롯 lock (KST 평일 21-24시) + group chat operational + Week 1 (5/11~5/17)에 협업 방식 trial-and-error 허용.

---

## Appendix D. Phase 2 / Track 2 핸드오프
*Phase 2 / Track 2 Handoff*

Track 1.5 launch (2026-06-03 이후) 검토 항목.

### D.1 자동화 확장 (v2 carry)
- AI assist drafting → publish 자동화 검토 (gate는 유지)
- Canonical Source Graph 자동 lookup 정확도 metric 측정 + 개선
- 시간 순서 / 출처 성격 메타 자동 분류 정확도 측정

### D.2 Track 2 (2027 대선) 학습 (v2 carry, v3 — 흐름 매체 emphasis)
- Track 1.5 사용 데이터 분석 (어떤 이슈·카드·Weekly Flow가 가장 공유됐나)
- 6명 팀 retro: daily intensive cadence 지속 가능성
- ASR / 비디오 콘텐츠 분석 추가 검토 (v1 deferred 항목)
- 정치인 drill-down 뷰 추가 검토 (v1 C 단계 확장)
- KG ontology 학술 정리 + 논문 발표 가능성
- **흐름 누적 → Knowledge Graph 진화** (현미 vision §1.3 (d)): 정치 백과사전 / 정치인 발언 archive / 정책 이슈 timeline

### D.3 사용자 경험 개선 (v2 carry)
- 카드 graph view (claim propagation 시각화)
- 시간 축 view (이슈 진화 추적, Weekly Flow의 확장)
- 채널 cluster view (진영별 다룬 채널)
- 검색 / 필터 기능 (이슈·기간·후보)
- 사용자 피드백 mechanism (제보 / 정정 요청)

### D.4 거버넌스 확장 (v2 carry)
- 한국 변호사 ongoing retainer 검토
- 외부 advisor council (학계·언론·시민사회) 구성 검토
- Bug bounty / public correction protocol

### D.5 데이터 소스 확장 (v3 갱신)
- data.go.kr 추가 dataset 탐색 (v3 Track 1.5에서 시작한 초기 활용 확장)
- BIGKinds API 재검토 (Track 2 시점 비용·약관 변화 시)
- 추가 정부 open data portal (지자체별 open data, 통계청 microdata 등)

---

## Canonical References
*Canonical References*

- `PRD.md` (v1.0) — Strategic spec, 2026-04-19 lock. v3의 mission statement 출처.
- `PRD-v2.md` (v2.0) — Operational spec, 2026-05-05 (NYC 새벽). v3의 운영 layer 기반.
- `archive/PLAN_v0_superseded.md` — Historical draft.
- `docs/deck.html` — Recruitment deck.
- `docs/mock-dashboard.html` — Mock dashboard.
- `docs/recruitment-onepager.md` — Recruitment one-pager.
- `~/vault/sources/2026-05-05-prism-jaehoon-call.md` — 5/5 재훈 콜 정제본 (v3 결정 source).
- `~/.claude/projects/-Users-jackkang/memory/project_prism-provenance.md` — Project status memory.

---

## Document Metadata

**Version**: v3.0
**Last updated**: 2026-05-06 (NYC)
**Authors**: Jack Kang (강동재) + Claude (writing assist, all decisions Jack-led)
**Source decisions**: 2026-05-05 [[김재훈]] 콜 (1.5h, KST 14:24~15:53) + 2026-05-05 [[김현미]] 1차 PRD 인계 미팅 (KST 16:00~)
**Approval**: Pending Jack final review + 5/11 6명 팀 walkthrough.
**Update protocol**: PRD-v3 본문 변경은 6명 팀 합의 + 명시적 commit message 필수. Appendix는 Jack 단독 update 가능.

---

*End of PRD v3.0*
