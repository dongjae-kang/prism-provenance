# PRISM Provenance — PRD v3 Core

*한국 정치 정보가 어떻게 변형되며 퍼지는지를 매일 추적하는 흐름 매체*

**Status**: Design locked (2026-05-05)
**Launch**: 2026-05-30 (target)
**First application**: 2026-06-03 지방선거 (Track 1.5)

이 문서는 **product spec**이다. 결정 reasoning, 거부된 alternatives, 운영 risk 등 의사결정 history는 [`PRD-v3.md`](./PRD-v3.md) (상세본) 참조.

---

## 1. 한 줄 정의

**PRISM은 한국 정치 YouTube에서 클레임이 어디서 시작되어 어떻게 변형되며 퍼지는지를 매일 추적하고, 그 흐름과 출처를 사용자가 직접 확인할 수 있도록 보여주는 흐름 매체다.**

---

## 2. Problem

한국 정치 YouTube에서 misinformation이 빠르게 확산되지만, 어떤 콘텐츠가 어디서 시작해 어떻게 퍼지는지 체계적으로 확인할 수 있는 공공 인프라가 없다. 2026년 6월 지방선거는 이 공백이 실제 투표 행동에 영향을 미치는 고위험 구간이다.

가장 가까운 시도들은 모두 다른 layer:
- 팩트체크 도구 (SNU팩트체크·JTBC팩트체크·뉴스타파): True/False 라벨링 + 사후 + 느림
- 뉴스 큐레이션 (뉴닉·슬로우뉴스·피렌체의 식탁): 사회 일반 + 흐름 안 보여줌
- ChatGPT/Perplexity: Korean political graph 없음 + hallucination

**한국에 "정보가 어떻게 변형되며 퍼졌나"를 매일 다루는 매체가 없다.** PRISM이 채우는 white space.

---

## 3. Target Users

### 1차: 정치 관심 minority (전체 유권자 5-15%)
- 이미 정치 YouTube 소비 + misinformation 신호에 민감
- "이 영상이 어디서 시작된 이야기인지" 확인하고 싶은 동기
- 카톡으로 정치 콘텐츠 공유 습관

### 2차 (이슈 framing으로 진입): 정치 관심 적은 20-30대
- 본인 삶 직결 이슈 (주거·노동·AI 규제 등)로 진입
- 이슈 라벨 시스템이 매개

### 3차: 언론·연구자 (post-launch organic)

**처음부터 mass를 노리지 않는다.** Minority의 자발 사용 + 카톡 공유 → D-7~D-1 자연 mass 확장.

---

## 4. What We Make

### 4.1 평일 카드 (월~금 매일 5장)

모바일 우선, 카톡·X 공유 최적화. 구성 (필수 12 + 조건부 2):

1. **이슈 라벨** — 진입 식별자 (예: "주거")
2. **N일째 추적 카운터** — 카드 좌상단 ("📍 14일째 추적 중")
3. **카드 제목 — 수치 trajectory 포함** — 변형 폭이 제목에 직접 (예: "청년 무주택 통계: 35% → 50% → 60% (3일간)")
4. **카드 hero — 수치 trajectory 시각화** — 제목 아래 mini chart로 변형 폭 visible
5. **본문 (200-400자)** — factual 기술
6. **클레임 병렬 제시** — 2-4개 클레임 (발화자·발화일·발언)
7. **시간 순서 메타** — "가장 먼저 등장 / 24시간 후 / 익일 변형 / 주말 동안 확산"
8. **출처 성격 메타** — "정치인 직접 / 언론 인용 / 익명 커뮤니티 / 2차 가공"
9. **Canonical source 병렬** — 통계청·부처·후보 공식·data.go.kr 등 1-3개
10. **출처 링크 (전부)** — 영상 + 원본
11. **Provenance footer** — "PRISM은 발언을 분류하지 않습니다. 출처를 보여드립니다. 판단은 사용자 본인의 몫입니다. 사람이 검수했습니다."
12. **공유 버튼** — 카톡, X, 링크 복사

**조건부 (검토 후 채택):**
- (a) **영상 썸네일** — 각 클레임 옆에 YouTube 썸네일. Recognition moment. 저작권·캐싱 김재훈 검토.
- (b) **분광 아이콘** — 카드 좌상단 작은 PRISM 분광 SVG. 디자인 quality bar 통과 시만.

**카드 예시 (hook layer 포함):**

```
◇ 14일째 추적 중                      [이슈 라벨: 주거]

청년 무주택 통계: 35% → 50% → 60%
3일간의 변형

[hero 시각화 — 변형 trajectory]
35% ●━━━ 38% ●━━━ 50% ●━━━ 60% ●
통계청    국토부    후보X    채널Z

이번 주 정치 YouTube에서 청년 무주택 통계 인용이
네 가지 수치로 등장했습니다.

[클레임]
1. 후보 X (4/27 영상)            [영상 썸네일]
   • 발언: "청년 50% 무주택"
   • 시간: 가장 먼저 등장
   • 출처 성격: 정치인 직접 발언

2. 후보 Y (4/28 토론회)          [영상 썸네일]
   • 발언: "청년 38% 무주택"
   • 시간: 24시간 후
   • 출처 성격: 정치인 직접 발언

3. 정치 채널 Z (4/29 영상)       [영상 썸네일]
   • 발언: "청년 무주택 60% 시대"
   • 시간: 익일 변형
   • 출처 성격: 2차 가공 채널

[관련 canonical source]
- 통계청 2025 인구주택총조사: 청년 35% 무주택
- 국토부 2025 보고서: 19~34세 무주택 38%
- data.go.kr 청년주거실태 dataset

[출처 링크 — 전부]

PRISM은 발언을 분류하지 않습니다.
출처를 보여드립니다.
판단은 사용자 본인의 몫입니다.
사람이 검수했습니다.

[카톡 공유] [X 공유] [링크 복사]
```

**톤:** 표준 존댓말 + factual + 인간미. 캐릭터 voice X, 위트 X, 풍자 X. 정치 평가 표현 ("우려된다", "심각하다") 금지.

### 4.2 토요일 Weekly Flow (주 1장)

그 주 가장 활발히 변형된 클레임 1개를 깊게 추적:
- 한 주 평일 카드 25-30장 중 가장 활발한 클레임 1개 선정
- 시간 축 timeline (월요일 등장 → 화요일 변형 → 수요일 확산 → 목요일 카운터 → 금요일 진정)
- 각 단계에 영상 link + canonical source 비교
- "이번 주의 흐름" 헤더로 흐름 매체 정체성 직접 surface
- 토요일 오전 9시 드롭 (주말 카톡 peak)

평일 카드를 raw material로 재활용 → 신규 production 부담 최소화.

### 4.3 "How We Work" 페이지 (Process Transparency Disclosure)

인간이 어디에서 어떻게 검수·관리하는지 절차 자체를 공개:

- 우리는 무엇을 하지 않는가 (5가지 negative definition)
- 우리는 무엇인가 (흐름 매체)
- 매일의 작업 흐름 (daily workflow 그대로)
- 검수 체크리스트 (approver checklist 6개)
- 우리가 신뢰하는 출처 (canonical 7종 + data.go.kr)
- 우리가 사용하는 AI (자동화 항목 + 자동화 안 하는 항목)
- 책임자 (Jack 실명 + 역할 단위 팀)
- PRD 변경 history (GitHub public repo)
- 정정·제보 channel

**노출 방식**: 사이트 navigation에 명시 + footer 링크. 진입 modal 강제 X.

**Dual purpose**: (1) 객관성 추구 의도를 절차 가시화로 implicit signaling, (2) 사전 자기 보호 (legal alibi). 사후 변명이 아니라 사전 표명.

### 4.4 "Show, don't classify" 원칙

PRISM은 어떠한 평가 라벨도 부여하지 않는다.

- ❌ "왜곡됨", "거짓", "오해 소지", "True/False/Misleading", "사실과 다름", "검증됨"
- ✅ "다음과 같은 통계가 있습니다", "통계청 자료에 따르면", "오마이뉴스 4월 28일 기사에 따르면 발언이 있었다고 합니다" (*보도 사실* 인용)

**왜:**
1. 법적 risk 최소화 (명예훼손법 + 공직선거법 250조·82조의4는 모두 *판단·평가 진술*에 적용)
2. 사용자 자율성 존중 — 가르치는 톤 0
3. 운영 부담 감소 — daily intensive cadence 가능

### 4.5 Hook & Felt Pull layer

정치 관심 적은 2030 secondary target의 felt pull을 의도적으로 높이는 layer. **재미가 아니라 substance의 surprise + 자기 정체성 신호**. 흐름 매체 정체성 (§1)과 같은 axis 위에 있는 hook만 채택.

**채택된 hook 8개:**

1. **카드 hero 수치 trajectory** (§4.1) — 제목 + mini chart에 변형 폭 직접 노출 ("35% → 50% → 60%"). Scroll에서 즉시 인지.

2. **N일째 추적 카운터 (3-layer 통합)**
   - Site-level: 사이트 홈 헤더 ("추적 중인 클레임 24개, 가장 오래된 것 21일째")
   - Card-level: 카드 좌상단 라벨 ("📍 14일째 추적 중")
   - Weekly Flow-level: 토요일 카드 핵심 지표 ("이 클레임 N일째 N차 변형")
   - Backend schema에 클레임 ID + 시작일 + 변형 history 박힘 필수.

3. **사이트 hero — "오늘의 흐름" Radial Spread 시각화**
   - 진입 즉시 보이는 첫 visual element. 카드 list가 아니라 흐름 자체가 첫 인상.
   - 중심 = canonical source, 바깥 = YouTube 변형 클레임. 분광 메타포와 시각적 일치.
   - 색상 lock: 무지개 X (진영색), monochrome / 단일 cool tone / 출처 성격별 mapping 중 하나.
   - A안 (Sankey) vs B안 (Radial) — 데모 단계에서 prototype 비교 후 quality 우월 안 final.

4. **PRISM 분광 메타포 — Design Language (조건부 채택)**
   - Logo + 사이트 hero + 카드 hero corner + brand voice ("이번 주의 스펙트럼")
   - **Quality bar 통과 시만 채택.** 미달 시 drop, 단순 typography brand identity로 fallback.

5. **사이트 hero 한 줄 lede (매일 갱신, factual only)**
   - 신문 1면 lede 효과. 큰 글자 한 줄.
   - 좋은 예: "오늘 청년 무주택 통계 인용이 네 가지 형태로 등장했습니다."
   - 평가 표현 금지.

6. **Weekly Flow 리더보드 (사실 metric only)**
   - "이번 주 가장 멀리 간 클레임 / 가장 빠르게 변형된 / 가장 많이 인용된 source"
   - 평가 ranking 금지 ("가장 왜곡된" 등 X).

7. **카톡 미리보기 한 줄 hook**
   - 큐레이션 워크플로우에 "공유 한 줄" 항목 추가. 본문과 별개로 카톡 description 슬롯.
   - factual only. 평가 표현 금지.

8. **정정 요청 채널** (§4.7로 분리, 신뢰 loop)

**거절된 hook:**
- 사용자 제보 → 운영 부담 + identity 미스매치 (PRISM은 추적 매체, 수집 매체 X)
- 카톡 채널 weekly push → Track 1.5 scope 외, post-launch deferred
- 무지개 색상 시각 메타포 → 진영색 회피 절대 lock

**Pre-launch backfill 필수 (5/24~5/29):** 시각화 + 카운터가 launch day 빈약하지 않으려면 시드 클레임 20-30개 backfill 작업 필요. Daily cadence 5장 → 4장 일시 축소 허용.

### 4.6 정정 요청 채널

사용자가 카드 사실 오류 발견 시 정정 요청. Form 항목: 카드 URL + 정정 부분 + 근거 출처 link + (선택) 연락처. 24h 내 큐레이터 검토 + 응답. 검증된 정정은 retraction protocol에 따라 카드 revision (revision history visible).

신뢰 loop: "사람이 검수했다 + 사용자도 정정 가능". Process Transparency Disclosure (§4.3)와 같은 axis.

---

## 5. Sources & Method

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

**작동:**
```
[정치 YouTube 클레임 surface]
  → [Graph 조회 — 인물·기관 식별]
  → [관련 노드 canonical source 즉시 lookup]
  → [카드 본문에 병렬 제시]
```

### 5.3 data.go.kr (공공데이터포털)

무료 키 활용:
- 국회 속기록·회의록 (정치인 발언 1차 출처)
- 정책브리핑 (정부 정책 archive)
- 선거 정보지 (선관위 공식 데이터)

**API 키 신청 진행 (5/10 PRD 확정 전).**

BIGKinds API 사용 안 함 (비용·약관·본문 제한 한계).

### 5.4 정치 YouTube monitoring

- 시드 채널 40-60개 (좌·우 균형)
- YouTube 트렌딩 정치 카테고리 일일 review
- 후보자명 쿼리로 누락 보강
- 22개 신호 수집 (API + 파싱 + AI 파생)
- 크롤링 안 함 (법적 risk + 안정성)

---

## 6. Operations

### 6.1 일일 워크플로우

```
오전 8:00  큐레이터 lead (Jack) data.go.kr + YouTube monitoring 결과 review
오전 8:30  AI assist로 trending claims 자동 추출 → 큐레이터 검토 큐
오전 9:00  큐레이터 lead가 그날 카드 후보 10-15개 선정
오전 9:30  AI assist로 카드 초안 자동 작성
오전 10:00 큐레이터 lead가 5장 final 선정 + 인간미·톤 가다듬기
오전 11:00 다른 팀원 1명이 publish gate review
정오 12:00 5장 publish + 카톡 공유 trigger
```

**토요일** (Weekly Flow):
```
오전 7:00  Lead가 한 주 카드 25-30장 review
오전 8:00  가장 활발히 변형된 클레임 1개 선정
오전 8:30  Weekly Flow 카드 작성 (시간축 + 변형 timeline)
오전 9:00  Approver review + publish
```

### 6.2 1 human approver gate

> PRISM card는 최소 1명의 human approver의 명시적 approval 없이 publish 되지 않는다.

**Approver checklist (6개):**
1. 클레임 원본 영상 link 작동 + 발언 정확성
2. Canonical source 원본 link 작동 + 인용 정확성 (data.go.kr 포함)
3. 분류 라벨 포함 여부 (§4.4 위반 감지)
4. 정치 평가 표현 포함 여부 (톤 가이드라인 위반 감지)
5. 후보 평가·투표 추천 함의 포함 여부
6. 명예훼손 위험 평가

6개 모두 통과 + approver 명시적 approval → publish.

**Self-approval 금지.** Approver 이름은 카드 metadata에 기록 (사후 추적, 사용자 facing 안 함).

### 6.3 AI 활용

**자동화 (AI):**
- data.go.kr / Canonical Source Graph 자동 lookup
- YouTube trending claims 자동 추출
- 카드 초안 자동 작성 (claim + source 병렬)
- 시간 순서 / 출처 성격 메타 자동 분류 (큐레이터 검수)
- 톤·스타일 일관성 check
- 분류 라벨 위반 자동 감지
- 카톡 미리보기 메타데이터 자동 생성

**자동화 안 함 (사람):**
- 그날 publish할 5장 (+ 토요일 1장) final 선정
- 카드 톤·인간미 최종 가다듬기
- 시간 순서 / 출처 성격 메타 최종 검수
- Publish gate approval
- Risk 판정 (명예훼손·정치 평가)

---

## 7. What We Are Not (5가지)

PRISM의 정체성 lock. 외부 압력에 대한 1차 응답 자료.

1. **사회·경제 zeitgeist 도구가 아니다.** 정치와 무관한 사회·경제 이슈 자체는 다루지 않는다.
2. **후보 평가 도구가 아니다.** "이 후보가 좋다 / 나쁘다" 판단을 제공하지 않는다.
3. **팩트체크 도구가 아니다.** True/False 라벨링하지 않는다. 명시는 사이트 진입 시 사용자 facing 노출.
4. **투표 추천 도구가 아니다.** 투표소 알림 등 선거기 한정 기능도 추가하지 않는다.
5. **분류 도구가 아니다.** Show, don't classify.

---

## 8. Team

| 역할 | 담당 |
|---|---|
| Lead + 큐레이션 큐레이터 | Jack Kang (강동재) |
| Backend / pipeline 리드 | 김재훈 (개발 리딩 + 합류 멤버 협업 방식 결정 권한) |
| Governance + 이슈 큐레이션 보조 | 정아인 |
| 디자인 / UX 리드 | 김현미 |
| Provenance UI / 디자인 | Saetbyeol Leeyouk (MIT Media Lab) — **5/11 본격 합류** |
| Frontend / 개발 | 샘 리 (Sam Lee, EPFL Computer Science) — **5/11~ 본격 시작** |

6명 모두 학업·직장·다른 프로젝트 병행. 일정은 마일스톤 단위.

**Jack daily commitment**: launch 기간 (5/4 ~ 6/3) 매일 lead 큐레이션 직접 담당. 학업 충돌 시 사전 공지 + 백업 큐레이터 (정아인) 지정.

**정기 미팅 슬롯**: KST 평일 21:00–24:00 (= NYC 오전 8:00–11:00).

---

## 9. Milestones

| 시점 | 마일스톤 |
|---|---|
| **2026-05-10 (토)** | PRD v3 확정 + Claude Code 데모 빌드 + 김현미와 디자인 디테일 1차 점검 |
| **2026-05-11 (월)** | Saetbyeol + Sam Lee 합류. 6명 team kick-off |
| **2026-05-17 (일)** | Backend MVP demo + 카드 form factor 사이트 통합 + "How We Work" 페이지 초안 |
| **2026-05-24 (일)** | Soft launch 준비 (beta 5-10명) + 한국 변호사 preliminary check |
| **2026-05-30 (토)** | **Public launch.** Daily 5장 cadence 시작 |
| **2026-06-01 ~ 6/2** | Election week escalation (daily 7-10장) |
| **2026-06-03 (수)** | **선거일.** 실시간 monitoring + 사후 정리 |
| **2026-06-04 ~ 6/10** | Mandatory rest week + retro |

---

## 10. Launch Readiness Checklist

Public launch (5/30) 전 필수 통과:

- [ ] PRD v3 확정 (5/10)
- [ ] Daily 5장 cadence 1주 연속 안정 운영
- [ ] Weekly Flow 토요일 1장 첫 발행 성공 (5/24 또는 5/31)
- [ ] Approver gate 모든 카드 적용 + checklist 6개 통과율 100%
- [ ] 분류 라벨 위반 0건 (지난 1주)
- [ ] 카톡 미리보기 정상 작동 (5개 device 테스트) + 카톡 한 줄 hook 모든 카드 작성
- [ ] Canonical Source Graph 후보자 매핑 ≥80% 완성
- [ ] data.go.kr API 키 발급 + 자동 lookup 작동
- [ ] About 페이지 + "How We Work" 페이지 노출
- [ ] 한국 변호사 preliminary check 완료
- [ ] 8개 이슈 카테고리 MECE 재검증 완료
- [ ] 6명 팀 burn-out 신호 monitoring (weekly retro)
- [ ] 사이트 hero "오늘의 흐름" 시각화 작동 + 시드 클레임 backfill 완료
- [ ] N일째 카운터 site/card/Weekly Flow 3-layer 모두 작동
- [ ] 분광 메타포 디자인 quality bar 통과 또는 drop 결정
- [ ] 카드 hero 수치 trajectory 시각화 작동
- [ ] 사이트 hero 한 줄 lede 매일 갱신 워크플로우 정착
- [ ] Weekly Flow 리더보드 form factor 작동
- [ ] 정정 요청 form 작동 + 24h retraction protocol 연결
- [ ] (조건부) 영상 썸네일 저작권/캐싱 검토 완료 + 채택/거절 결정

---

## 11. Differentiation (한 줄)

> **PRISM은 한국 정치 정보가 어떻게 변형되며 퍼졌나를 매일 추적하는 흐름 매체다.** Provenance를 분류 없이, 시간 순서 + 출처 성격 메타와 함께, canonical source graph + data.go.kr 기반으로 제공한다. 흐름 매체 정체성을 매일 visible한 hook (시각화 hero + N일째 카운터 + 분광 메타포) 으로 surface. 한국에 동일 layer의 매체 부재 — white space.

| 도구 | 강점 | PRISM 대비 약점 |
|---|---|---|
| ChatGPT / Perplexity | Universal Q&A, 빠름 | Korean political graph 없음, hallucination, 흐름 안 보여줌 |
| 뉴닉 | Daily news, 가벼운 톤 | 사회 일반, 정치 미스인포 specific X, 흐름 안 보여줌 |
| 슬로우뉴스 | 깊은 시사 분석 | Weekly+ cadence, 흐름 안 보여줌 |
| SNU팩트체크 / JTBC팩트체크 / 뉴스타파 | 깊은 fact-check | True/False 라벨, 사후, YouTube 분석 안 함, 흐름 안 보여줌 |

**우리는 정보가 어떻게 움직였나 (how it moved) — 한국에 prior art 없음.** 흐름 매체 정체성을 시각화·카운터·메타포로 사용자가 매일 *느낄 수 있게* 한다.

---

## 12. Tech Stack (high-level)

```
[YouTube Data API v3 + yt-dlp 백업]
        ↓
[Discovery Cascade: 시드 채널 + 트렌딩 + 후보자명]
        ↓
[22-signal Collection + AI 파생]
        ↓
[Canonical Source Graph + data.go.kr cross-reference]
        ↓
[Claim Matching + 시간 순서 / 출처 성격 메타 자동 분류]
        ↓
[AI assist 카드 초안 → 큐레이터 final → Approver gate]
        ↓
[Public Web (반응형, 카톡 공유 최적화)]
```

**Web only** (앱 X). 로그인·personalization 없음.

세부 기술 선택 (KG DB / embedding 모델 / LLM / frontend stack)은 개발 리드 (재훈) 위임.

---

## 13. Constraints & Risks (요약)

**Legal**: 공직선거법 82조의4 + 250조 + 한국 명예훼손법 — 모두 *판단·평가 진술*에 적용. Show-don't-classify가 가장 큰 mitigation. 한국 변호사 preliminary check 필수.

**Operational**: 6명 모두 part-time. Daily cadence + 토요일 Weekly Flow의 burn-out risk. Approver rotation + 일요일 휴식 + Election week 후 mandatory rest week로 mitigation.

**Product**: 8개 이슈 카테고리 MECE 부족 가능성 (5/10 전 재검증). Process Transparency Disclosure가 "AI 자동화 못 한다" 신호로 역효과 가능성 (페이지 framing으로 mitigation).

자세한 risk + mitigation은 [`PRD-v3.md`](./PRD-v3.md) Appendix C 참조.

---

## Document Metadata

**Version**: v3.0 Core (with Hook Layer)
**Last updated**: 2026-05-06 (NYC, hook layer integrated)
**Authors**: Jack Kang + Claude (writing assist, all decisions Jack-led)
**Source decisions**: 2026-05-05 [[김재훈]] 콜 + 2026-05-05 [[김현미]] 1차 PRD 인계 미팅 + 2026-05-06 hook layer brainstorm session (2030 secondary target felt pull)
**Companion document**: [`PRD-v3.md`](./PRD-v3.md) — 결정 reasoning, 거부된 alternatives, risk 상세

---

*End of PRD v3 Core*
