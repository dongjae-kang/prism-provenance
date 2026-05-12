# PRISM 데이터 소싱 계획 — Trending Validation Layer

**작성일**: 2026-05-12
**작성자**: 강동재 (Jack Kang) with Claude Code
**상태**: 팀 공유용 working doc. 재훈 검토 후 PRD-v4 §5.6 patch로 흡수 예정.
**관련 PRD 섹션**: v4 §5 (Sources & Method), §5.3 (data.go.kr), §5.4 (YouTube), §5.5 (매체 3시각 cross-reference), §6.1 (일일 워크플로우)
**관련 회의**: 2026-05-05 재훈 콜 (data.go.kr 1차 lock, 빅카인즈 본격 API reject), 2026-05-11 카톡방 샘님 우려 제기

---

## TL;DR (3줄 요약)

1. **모든 source는 (a) ToS 명시 확인, (b) 공식 API path 존재, (c) 합법 path 사용 — 3-step 검증을 거쳐야 한다**는 governance 원칙을 PRD에 lock.
2. 검증된 합법 path 4종 조합으로 PRD 데이터 요구사항 충족: YouTube Data API v3 (1차) + 네이버 검색 API + data.go.kr + Google Trends API (2차) + 네이버 DataLab (단기 보조, 2026-07-23 자동 deprecate).
3. 7개사 직접 스크래핑·문의, 빅카인즈 트렌딩 페이지 직접 스크래핑, Google Trends 페이지 스크래핑은 명시적 reject.

---

## §1. 문제 진단 — 왜 기존 path가 안 되는가

### 1.1 7개사 직접 RSS·문의 발송 path의 한계

5/11 카톡방에서 샘님가 제기한 우려:
- 대부분 언론사가 RSS·콘텐츠의 "개인적 목적 이외 사용"을 약관상 금지.
- 한겨레는 봇 접근 명시 차단 (우회만 가능 = ToS 위반).
- 경향·국민일보 등은 문의 시 가능성 있으나 매체별 답변 timeline 불확실.

**판단**: 7개사 각각에 문의·협의를 거치는 path는 기간이 오래 소요되고, 그것이 가장 좋은 방법이라는 확신도 부족한 상태. 확신이 약한 path에 시간과 노력을 투입하는 것이 본 프로젝트에 부적합하다고 판단. 더 확실하고 합법성이 명확한 다른 path를 찾는 것이 우선.

### 1.2 빅카인즈 본격 API path의 한계 (5/5 reject 유지)

5/5 재훈 콜에서 빅카인즈 본격 API 사용을 거부한 사유:
- **가져올 수 있는 기사·언론사가 한정적**.
- **지방 언론사 위주로 분포** (조선·중앙·동아·한겨레·경향 등 전국 영향력 매체의 cover가 약함).

본 계획에서도 이 reject 결정은 그대로 유지. 단, 빅카인즈 ecosystem 안에 다른 합법 path (공공데이터포털 경유)가 별도로 존재하며, 그것은 활용 가능 (아래 §3.3).

### 1.3 단순 스크래핑 path는 모든 source에서 reject

검증 결과:
- 네이버 뉴스 페이지 스크래핑 → 네이버 ToS 위반.
- 빅카인즈 트렌딩 페이지 스크래핑 → 빅카인즈 약관 "크롤링 등 부정한 데이터 수집 금지" 명시 위반.
- Google Trends 페이지 스크래핑 → Google ToS "scraping, building databases, creating permanent copies" 위반.
- Daum/Nate most read 페이지 스크래핑 → 공식 API 부재 + ToS 미확인.

**즉 "긁어오기" 자체가 본질적 risk**. ToS 주체가 어디든 동일.

---

## §2. 해결 원칙 — Governance

### 2.1 일관 검증 원칙 (모든 source 적용)

PRISM이 활용하는 모든 외부 데이터 source는 아래 3-step 검증을 통과해야 한다:

| Step | 검증 항목 |
|---|---|
| (a) | 해당 source의 ToS·이용약관·robots.txt에서 자동화·재배포·DB 저장에 대한 명시 정책 확인 |
| (b) | 공식 API·공개 데이터셋·RSS 표준 등 sanctioned path 존재 여부 확인 |
| (c) | sanctioned path만을 통해 활용 (직접 스크래핑은 default reject) |

**Why**: "공공기관 운영" ≠ "자동 OK". "공공 서비스 = 자유 활용 가능"이라는 직관은 약관 앞에서 무효. 샘님의 5/11 우려는 단일 source 우려가 아니라 governance 원칙으로 일반화되어야 한다.

### 2.2 매체별 Attribution 보존 — Layer-conditional 원칙

PRISM이 활용하는 모든 source 데이터는 매체명·발행시점·URL을 보존한다. 단 노출 layer에 따라 표현 방식이 갈린다.

**Substantive layer (매체별 명시 필수)**:
- 카드 본문, 시간 순서 메타 (§4.1 #7), Canonical source 병렬 (§4.1 #9)
- 매체 3시각 비교 (§4.7)
- 흐름 7 step (§4.8)
- Weekly Flow 리더보드 (§4.2)
- 작업 노트, DB schema, 큐레이터 검토 화면

→ "조선닷컴 4/27 09:14, 연합뉴스 4/27 13:42, 한겨레 4/28 08:30" 식 매체명·발행시점 명시.

**Summary layer (aggregation 허용, drill-down 보장)**:
- 사이트 hero 한 줄 lede (§4.5 hook #5)
- N일째 카운터 (§4.5 hook #2)
- 이슈 카테고리 chip (§4.0)
- 피드 카테고리 group 헤더

→ "네 갈래로 흘러갔어요", "추적 중인 클레임 24개" 같은 aggregation OK. 단 클릭/탭하면 substantive layer로 진입 가능해야 함 (drill-down).

**금지**: substantive layer에서 "여러 매체", "다른 매체들", "주요 언론들" 같은 vague aggregation. 이건 layer 혼동이 아니라 attribution 회피.

**Why**: PRD §4.4 "Show, don't classify"가 사용자 노출 원칙이라면, 본 원칙은 같은 axis의 데이터 파이프라인 원칙. 사용자가 자력 추론하려면 substantive layer detail에 접근 가능해야 함. drill-down이 보장되면 summary layer aggregation은 "Show" 원칙을 깨지 않는다.

### 2.3 PRD §4.3 (Process Transparency Disclosure)와의 연결

본 governance 원칙은 PRD §4.3 "How We Work" 페이지의 "5. 우리가 신뢰하는 출처" 섹션에 명시적으로 surface해야 한다. 단순 "출처 list"가 아니라 "어떤 합법 path로 받아오는가"까지 공개. legal alibi + 사용자 신뢰 동시 강화.

---

## §3. 검증된 합법 Path — 4종 (+ 단기 보조 1종)

### 3.1 [1차 Discovery] YouTube Data API v3 — PRD §5.4 lock 유지

| 항목 | 내용 |
|---|---|
| 용도 | 정치 영상 발견 + 22 신호 수집 (제목·설명·태그·썸네일·자막·댓글·view·channel 메타 등) |
| 합법성 | ✅ Google 공식 API (sanctioned path) |
| 비용 | 일일 quota 무료, 초과 시 paid tier |
| 통합 위치 | PRD §5.4 + §12 tech stack 기존 lock |
| 변경 사항 | 없음 |

### 3.2 [2차 매체 인용 검증] 네이버 검색 API (뉴스)

#### 기술 스펙

| 항목 | 내용 |
|---|---|
| 용도 (요약) | (A) YouTube 영상 클레임의 매체별 보도 추적 + (B) PRD §5.5 매체 3시각 cross-reference. 본문 ingest X, 매체별 attribution 보존 메타데이터만 활용. |
| 응답 필드 | `title`, `originallink` (언론사 직접 URL), `link` (네이버 뉴스 URL), `description` (snippet, `<b>` 태그 포함), `pubDate` |
| 합법성 | ✅ 네이버 Developers 공식 API (sanctioned path) |
| Rate limit | 일 25,000 호출 무료, 페이지당 max 100, start max 1,000 |
| 정렬 | `sort=date` (최신순) 또는 `sort=sim` (관련도) |
| ToS 주체 | 네이버 1개로 단순화 (7개사 직접 ToS 우회) |
| Coverage 검증 필요 | 한겨레의 일부 기사가 네이버 인덱스에 없을 수 있음 (네이버 input 정책 + 한겨레 자사 정책). 샘플 테스트로 실측. |
| 기존 MCP | `isnow890/naver-search-mcp` (third-party) — 평가 후 채택/거절 |

#### 본질 — "검색"이 아니라 "구조화된 인덱스 조회"

네이버 검색 API는 네이버 사이트 검색이 아니다. **네이버가 인덱싱한 7개사 기사 데이터베이스를 구조화된 형태로 조회**하는 path. 응답의 `originallink`가 조선닷컴·한겨레닷컴 등 언론사 직접 URL이라, 네이버는 인덱스 역할만 하고 실제 기사 자체는 언론사 사이트로 redirect된다. PRISM이 본문을 ingest·재배포하는 게 아님.

#### 활용 시나리오 A — 클레임 매체별 보도 추적

YouTube 영상에서 "통계청이 청년 무주택 비율 35% 발표했다고 함" 클레임 발견 시.

**1단계 — 쿼리 발사**:
- 키워드: `"청년 무주택" 35% 통계청`
- `sort=date` (최신순)
- `display=100` (페이지당)

**2단계 — 응답 (가상 예시, 실제 형식)**:
```
[
  {publisher: "조선닷컴",  pubDate: "2026-04-27 09:14", 
   title: "통계청 발표: 청년 무주택 35%, 5년 만에 10%p 상승",
   originallink: "https://www.chosun.com/.../2026/04/27/...",
   description: "통계청이 27일 발표한 자료에 따르면..."},
   
  {publisher: "연합뉴스",  pubDate: "2026-04-27 13:42",
   title: "[속보] 통계청 '청년 무주택 비율 35%'... 사상 최고",
   originallink: "https://www.yna.co.kr/...",
   description: "통계청이 발표한 청년 주거 실태조사..."},
   
  {publisher: "한겨레",    pubDate: "2026-04-28 08:30",
   title: "청년 무주택 35%, 주거정책 실패의 결과",
   originallink: "https://www.hani.co.kr/...",
   description: "전날 통계청 발표를 두고 야권은..."},
   
  {publisher: "경향신문",  pubDate: "2026-04-28 11:15",
   title: "통계청 통계, 청년 무주택 35%의 의미",
   originallink: "https://www.khan.co.kr/...",
   description: "..."},
   
  {publisher: "동아일보",  pubDate: "2026-04-28 18:00",
   title: "청년 무주택 35%... 정부 대응책은?",
   originallink: "https://www.donga.com/...",
   description: "..."}
]
```

**3단계 — PRISM 카드에 mapping**:
- 시간 순서 메타 (§4.1 항목 7): "조선닷컴 4/27 09:14 1차 보도 → 연합뉴스 4/27 13:42 wire → 한겨레 4/28 08:30 → 경향 4/28 11:15 → 동아 4/28 18:00"
- Canonical source 병렬 (§4.1 항목 9): 통계청 원문 발표 (data.go.kr 정책브리핑) + 위 5개 매체 originallink 병렬 표기
- 출처 링크 전부 (§4.1 항목 10): 각 매체별 originallink redirect

**핵심**: 모든 단계에서 매체명·발행시간·URL이 1대1로 attribution. "다른 매체들이 이어 인용" 같은 vague aggregation은 절대 사용하지 않는다.

#### 활용 시나리오 B — 매체 3시각 cross-reference (PRD §4.7)

같은 클레임에 대해 매체별 framing 비교 자료 retrieve.

**1단계 — 쿼리**: A와 동일 키워드. `display=100`으로 모든 매체 response를 한 번에 수집.

**2단계 — 매체별 그룹핑**:
- 조선닷컴: "5년 만에 10%p 상승" (정량적 변화 강조)
- 한겨레: "주거정책 실패의 결과" (정책 책임 framing)
- 경향: "통계의 의미" (해석·맥락 framing)
- 동아일보: "정부 대응책은?" (질문형 framing)
- 연합뉴스: "사상 최고" (역사적 기록 framing)

**3단계 — 큐레이터가 3개 선정**: 매체 다양성 + framing 대조 + PRD §4.7 "라벨 X, 매체명만 병치" 원칙에 따라 3개 final pick.

**4단계 — PRISM 카드 §4.7 섹션에 노출**:
```
"청년 무주택 35%" 통계에 대해

조선닷컴   "통계청 발표: 청년 무주택 35%, 5년 만에 10%p 상승"
한겨레     "청년 무주택 35%, 주거정책 실패의 결과"
경향신문   "통계청 통계, 청년 무주택 35%의 의미"

(각 헤드라인 옆 originallink redirect)
```

PRISM은 평가·라벨링 X. 사용자가 자력 추론.

#### 매체별 Attribution 보존

§2.2 layer-conditional 원칙을 본 source에 적용. 시나리오 A·B 모두 substantive layer (시간 순서 메타·Canonical source 병렬·매체 3시각·작업 노트·DB)에 해당하므로 매체명·발행시점·URL 명시 필수.

응답의 `originallink`가 매체별 직접 URL이라 attribution이 응답 시점에 이미 보존됨. 데이터 파이프라인 전 layer에서 잃지 않는 것이 핵심.

#### 한 가지 caveat — 인과 단정 금지

시나리오 A에서 "YouTube 영상이 그 특정 기사를 인용했다"고 100% 확정하기는 어렵다. 영상이 명시적으로 매체명·날짜를 언급하지 않는 한, 네이버 검색 API가 알려주는 건 "이 클레임을 다룬 매체들과 그 게재 시간"이지 "영상이 그 매체를 직접 봤다"는 인과는 아니다.

PRISM 카드는 "이 영상은 X 기사를 인용함" 형태로 단정하지 않고, "이 클레임에 대한 매체별 보도 (시간순)" 형태로만 surface. PRD §4.4 "Show, don't classify" 원칙으로 이미 cover됨.

#### 법적 근거 — 4축 검증

"네이버 검색 API를 통해 가져온 자료라 문제없다"는 주장은 단일 근거가 아니라 4-layer 보호 구조 위에 있다. 한 축이 약해도 다른 축으로 cover됨.

| # | 근거 | 핵심 내용 | 출처 |
|---|---|---|---|
| 1 | **네이버 Developers 약관 path** | 공식 API key 발급 + 약관 동의 = sanctioned access | [developers.naver.com](https://developers.naver.com/main/) · [AI·NAVER API Terms (PDF)](https://xv-ncloud.pstatic.net/images/provision/AI%C2%B7NaverAPIServiceTermsandConditions_1620716071502.pdf) |
| 2 | **저작권법 제7조 제5호** | "사실의 전달에 불과한 시사보도"는 저작권 보호 대상 X. 통계 수치·발표·발언 요지 등 단순 사실은 저작물성 X. 대법원 판례 일관. | [국가법령정보센터 저작권법](https://www.law.go.kr/LSW/LsiJoLinkP.do?lsNm=%EC%A0%80%EC%9E%91%EA%B6%8C%EB%B2%95) · [한국기자협회 헤드라인 저작권 해설](https://journalist.or.kr/m/m_article.html?no=51770) |
| 3 | **네이버 뉴스 제휴 구조** | 7개사 포함 981개 언론사가 뉴스제휴평가위원회를 거쳐 네이버 검색 노출을 sanctioned. originallink + title + snippet은 언론사가 네이버에 노출 허용한 메타데이터. | [포털 뉴스제휴평가위원회](https://newpotal.com/pages/page_149.php) · [네이버·카카오 뉴스 제휴 규정](https://www.mediaon.co.kr/bbs/bbs.html?mode=download&bbs_code=mo_notice&uid=28896&att_uid=8) |
| 4 | **저작권법 제28조 인용** | 공표된 저작물은 정당한 범위 + 공정한 관행 안에서 인용 가능. PRISM은 출처 명시 + originallink redirect + 본문 큐레이터 작성. | [한국저작권위원회 자료실](https://www.copyright.or.kr/) |

**PRISM 활용 형태 — 4축 모두 통과 조건**:
- 본문 ingest·재배포 ❌
- originallink redirect ✅ (외부 매체 사이트로 user 이동)
- 매체별 attribution (제목·발행시점·URL) 모든 layer에 명시 ✅
- "네이버 검색에서 가져옴" 공식 disclose ✅ (Process Transparency Disclosure §4.3)

**공식 disclose 권장 표현** (How We Work 페이지 §4.3 surface용):
> PRISM은 네이버 검색 API를 통해 7개사 매체 보도의 메타데이터(제목·발행시점·URL)를 retrieve합니다. 본문 자체는 각 매체의 원문 링크로 redirect되며, PRISM이 본문을 재배포하지 않습니다.

단순·명확. mis-represent ("네이버와 파트너십" 등) 금지.

### 3.3 [2차 매체 종합 트렌딩] data.go.kr "빅카인즈 오늘의 이슈 Top 10"

| 항목 | 내용 |
|---|---|
| 용도 | 매체 종합 트렌딩 cross-check (조중동·전국지 포함된 빅카인즈 분석 결과) |
| 합법성 | ✅ 공공데이터포털 정식 공개 파일데이터 (한국언론진흥재단 직접 게시) |
| 비용 | 무료 |
| 갱신 주기 | 매일 |
| Dataset ID | 15145668 |
| 통합 위치 | **PRD §5.3 (data.go.kr 1차 source lock) 활용 범위 확장**. 별도 path 신설 아님. |
| 핵심 distinction | 빅카인즈 **본격 API** (5/5 reject)와 다른 차원. data.go.kr 통한 공개 파일데이터는 독립적 합법 path. |

### 3.4 [2차 검색 트렌딩 cross-check] Google Trends API (2026 alpha)

| 항목 | 내용 |
|---|---|
| 용도 | 키워드 검색 트렌딩 + Related Queries + Interest Over Time. 글로벌 표준 신호 |
| 합법성 | ✅ Google 공식 API (alpha, sanctioned path) |
| 인증 | Google Cloud auth (alpha quota 제한) |
| 장기 안정성 | ✅ Track 1.5 + Track 2 (2027 대선) 모두 활용 가능 |
| 직접 스크래핑 | ❌ Google ToS 위반 ("scraping, building databases, creating permanent copies" 금지) |
| 통합 위치 | PRD §5.6 (신규) |

### 3.5 [단기 보조] 네이버 DataLab API — 2026-07-23 18:00 KST 자동 deprecate

| 항목 | 내용 |
|---|---|
| 용도 | 네이버 검색 트렌딩 + 성별·연령 분포. 한국 검색 점유율 1위 신호 |
| 합법성 | ✅ 네이버 공식 API |
| 비용 | 무료 (25k/day) |
| **종료일** | **2026-07-23 18:00 KST** (Naver 공식 공지) |
| 활용 window | Track 1.5 지방선거 기간 (5/30~6/3) 한정 |
| 통합 위치 | PRD §5.6 보조 항목. 7/23 이후 코드 자동 제거 schedule. |

---

## §4. Action Items — 3-tier 구조

### Tier 1 [즉시 실행 — 5/12~5/14]

> **신청 액션 (#1.1~#1.3) 담당자**: Jack이 시간을 내서 직접 발급받아 키를 팀에 전달. 재훈이나 샘님이 직접 발급받는 쪽을 선호하면 ownership 이전 가능. 무료 path라 누가 발급해도 비용·법적 issue 없음. 신청 절차 상세는 본 섹션 아래 "신청 절차 상세" 참조.

| # | Action | Owner | 완료 기준 |
|---|---|---|---|
| 1.1 | 네이버 검색 API 키 발급 신청 — 무료, 25k/day, 즉시 발급 | Jack (전달) | Client ID + Secret 발급 + 환경변수 lock |
| 1.2 | data.go.kr 활용 신청 + "빅카인즈 오늘의 이슈 Top 10" (Dataset 15145668) 1주일치 샘플 다운로드 — 무료, 즉시~1-2일 | Jack (전달) | 파일 형식·정치 cover·갱신 주기 실측 |
| 1.3 | Google Cloud 프로젝트 생성 + Trends API alpha 접근 권한 신청 — 무료, 승인 불확실 | Jack (전달) | alpha access 승인 또는 거절 통지. Launch 전 PRISM 전용 Google account로 이전 검토. |
| 1.4 | 네이버 검색 API 한겨레 coverage 샘플 테스트 (정치 키워드 5개 × 7일) | 재훈 또는 Jack | 한겨레 originallink 응답 비율 측정. 80% 미만 시 별도 fallback 검토 |
| 1.5 | `isnow890/naver-search-mcp` 평가 (보안·rate limit·메인테인 상태) | 재훈 | 채택/거절 결정. 거절 시 자체 wrapper로 진행 |

#### Tier 1 #1.1~#1.3 신청 절차 상세

추후 Jack 또는 다른 팀원이 발급 작업할 때 본 절차를 그대로 따라간다.

**#1.1 네이버 검색 API**
- 신청 URL: https://developers.naver.com/main/
- 절차: (1) 네이버 로그인 → (2) "Application 등록" → (3) 검색 API 활성화 → (4) Client ID + Client Secret 발급
- 비용: 무료 (일 25,000 호출)
- 소요: 즉시 (5~10분)
- 계정 귀속: 신청자 네이버 계정. 키는 환경변수로 공유 가능.

**#1.2 data.go.kr "빅카인즈 오늘의 이슈 Top 10" (Dataset 15145668)**
- 신청 URL: https://www.data.go.kr/data/15145668/fileData.do
- 절차: (1) 공공데이터포털 회원가입 → (2) 데이터셋 페이지에서 "활용신청" 또는 직접 다운로드 → (3) 활용 목적 기재 (해당 시) → (4) 자동 또는 1~2일 내 승인
- 비용: 무료
- 소요: 5분 신청, 승인 즉시~1-2일
- 핵심 caveat: 파일데이터는 회원가입만으로 즉시 다운로드 가능한 경우 많음. 실제 페이지에서 확인.

**#1.3 Google Trends API (2026 alpha)**
- 신청 URL: Google Cloud Console (https://console.cloud.google.com) → APIs & Services → Trends API
- 절차: (1) Google Cloud 프로젝트 생성 → (2) Trends API alpha access 요청 → (3) 승인 대기
- 비용: alpha 무료 (정식 출시 시 paid tier 가능성)
- 소요: 10분 신청, 승인 days~weeks (alpha라 불확실)
- 핵심 caveat: Alpha라 quota 매우 제한적. 승인 자체 거절 가능성. Fallback = trends.google.com 공식 페이지 user-facing 열람 (자동화 X) + 네이버 DataLab API (2026-07-23까지) 조합.
- 계정 귀속: Google Cloud 프로젝트 단위. 장기적으로 PRISM 전용 Google account/org로 이전 검토 권장.

### Tier 2 [데모·launch 통합 — 5/14~5/30]

| # | Action | Owner | 완료 기준 |
|---|---|---|---|
| 2.1 | 일일 워크플로우 (PRD §6.1 08:30) 자동화 스크립트에 4-source pull 통합 | 재훈 | YouTube + 네이버 검색 + data.go.kr + Google Trends 신호가 큐레이터 검토 큐에 한 화면으로 surface |
| 2.2 | "Trending signal triangulation" 시각화 (큐레이터 검토 화면) | 재훈 + 김현미 | YouTube 트렌딩 클레임에 대해 다른 3 신호의 cross-check 상태가 한눈에 보이는 UI |
| 2.3 | §5.5 매체 3시각 cross-reference 매칭 알고리즘 v1 (네이버 검색 API + LLM 보조) | 재훈 | 같은 클레임을 다룬 매체 3곳 자동 추천 → 큐레이터 final 선정 |
| 2.4 | Process Transparency Disclosure (§4.3) "우리가 신뢰하는 출처" 섹션 업데이트 — 합법 path 명시 | 정아인 + Jack | "How We Work" 페이지에 4-source + path 합법성 명시 노출 |

### Tier 3 [Long-term 운영 — 5/30 launch 이후]

| # | Action | Owner | 완료 기준 |
|---|---|---|---|
| 3.1 | 네이버 DataLab API 의존 코드의 7/23 deprecate scheduled task 등록 | 재훈 | 7/23 자동 제거 + Google Trends 단독 의존 전환 |
| 3.2 | Track 2 (2027 대선) 데이터 소싱 재검토 — DataLab 부재 가정 | Jack | 2026-Q4 PRD review 시 점검 |
| 3.3 | 새 source 추가 시 §2 governance 원칙 self-audit checklist 운영 (§2.1 ToS 3-step + §2.2 attribution layer 정합) | Jack + 정아인 | 모든 신규 source PR이 §2.1 + §2.2 통과 후 머지 |
| 3.4 | 매체 3시각 매칭 자동화 정밀도 운영 측정 → MVP 이후 자동 ratio 조정 | 재훈 + Jack | 큐레이터 manual selection 비율 추적 |

---

## §5. 명시적 Reject (변경 없음 + 본 계획에서 추가)

| Source / Path | Reject 사유 | 결정 시점 |
|---|---|---|
| 빅카인즈 본격 API | 가져올 기사·언론사 한정, 지방 언론사 위주 분포 (전국지 cover 약함) | 2026-05-05 재훈 콜 |
| 7개사 RSS 직접 활용 | 매체별 ToS 약관 risk + 7개사 개별 협의 timeline이 launch window 초과 (한겨레는 봇 차단 명시) | 본 계획 (2026-05-12) |
| 빅카인즈 트렌딩 페이지 직접 스크래핑 | 빅카인즈 약관 명시 금지 ("크롤링 등 부정한 데이터 수집 금지, API의 정당한 사용 필수") | 본 계획 (2026-05-12) |
| Google Trends 페이지 스크래핑 | Google ToS "scraping, building databases, creating permanent copies" 위반 | 본 계획 (2026-05-12) |
| Daum / Nate most read 페이지 스크래핑 | 공식 API 부재 + ToS 미확인 + 한국 시장점유율 낮음 (cross-check 가치 낮음) | 본 계획 (2026-05-12) |
| Twitter/X 한국 정치 트렌딩 | API 비용 + 봇 noise + Track 1.5 scope 외 | 본 계획 (2026-05-12) |
| 커뮤니티 (DC·FMK·더쿠 등) 정치 갤러리 활성도 | ToS·신뢰성 risk + identity mismatch (PRISM은 추적 매체, 커뮤니티 발원 추적 아님) | 본 계획 (2026-05-12) |

---

## §6. PRD-v4 통합 방안 — §5.6 신규 섹션 제안 골격

본 계획이 팀 검토를 거쳐 lock되면, PRD-v4.md / PRD-v4-core.md 양쪽에 아래 골격으로 §5.6 추가:

```
### 5.6 Trending Validation Layer

PRISM의 큐레이션이 한국 정보환경의 실제 트렌딩과 align되는지
검증하는 cross-check layer. 단일 source 의존 X, multi-signal triangulation.

5.6.1 1차 Discovery (PRD §5.4 lock 유지):
        YouTube Data API v3 — 정치 트렌딩 카테고리 + 시드 채널 40-60개

5.6.2 2차 Cross-check (합법 path only):
        (a) 네이버 검색 API — 7개사 매체 인용 검증 + 매체 3시각 cross-reference
        (b) data.go.kr "빅카인즈 오늘의 이슈 Top 10" — 매체 종합 트렌딩 신호
        (c) Google Trends API (alpha) — 검색 트렌딩 신호, long-term

5.6.3 단기 보조 (2026-07-23 자동 deprecate):
        네이버 DataLab API — 지방선거 기간 (5/30~6/3) 한정

5.6.4 Governance 원칙 (§2 본 계획):
        모든 source는 (a) ToS 명시 확인, (b) 공식 API path 존재,
        (c) 합법 path 사용 — 3-step 검증 필수.

5.6.5 명시적 Reject: §5 본 계획 표 그대로 PRD에 inline.
```

PRD-v4-core.md §5.6 + PRD-v4.md Appendix B (Reject 결정 reasoning) 양쪽 반영. Document Metadata "Last updated" 갱신.

---

## §7. Open Questions — 결정 필요

| # | Question | Decision Owner | Target Lock |
|---|---|---|---|
| 7.1 | `isnow890/naver-search-mcp` 채택 vs 자체 wrapper | 재훈 | 5/14 |
| 7.2 | Google Trends alpha 미승인 시 fallback (SerpApi paid? 또는 trends.google.com 공식 페이지 user-facing 열람만)? | Jack | alpha 결과 통지 후 |
| 7.3 | 한겨레 coverage 80% 미만 시 보완 path (한겨레 RSS 직접 문의는 timeline 미달, 다른 대안?) | Jack + 재훈 | Tier 1 #1.4 결과 후 |
| 7.4 | 매체 3시각 매칭 자동화 한계로 큐레이터 manual fallback 비율 얼마? | 재훈 + Jack | MVP launch 후 empirical |

---

## §8. 다음 단계

1. **재훈 검토** — 본 문서 GitHub repo `docs/data-sourcing-plan-0512.md` 머지 후 카톡방 공유. 재훈 회신 후 §7 Open Questions lock.
2. **PRD §5.6 patch 작성** — 재훈 lock 후 Jack이 PRD-v4.md / PRD-v4-core.md 양쪽 §5.6 추가 + commit.
3. **Tier 1 실행** — 5/14까지 4개 API key·access 모두 lock.
4. **데모 통합** — Tier 2 #2.1~2.4를 데모 v4 작업과 병행.

---

## Appendix — 검증 출처

| 항목 | 출처 |
|---|---|
| 빅카인즈 약관 "크롤링 금지" 명시 | bigkinds.or.kr 이용약관 |
| 한국언론진흥재단 빅카인즈 오늘의 이슈 Top 10 (Dataset 15145668) | data.go.kr/data/15145668/fileData.do |
| 네이버 검색 API 응답 필드 + 25k/day | developers.naver.com/docs/serviceapi/search/news |
| 네이버 DataLab 2026-07-23 종료 공지 | NCloud API docs |
| Google APIs ToS "scraping prohibited" | developers.google.com/terms |
| Google Trends API alpha 2026 출시 | Google for Developers + ScrapingBee 2026 가이드 |
| 네이버 통합 뉴스 랭킹 2020-10-22 폐지 | 한국일보 2020-10-23 보도 + 나무위키 |
| 5/5 빅카인즈 본격 API reject 결정 | `~/vault/sources/2026-05-05-prism-jaehoon-call.md` + PRD-v4.md §5.3 |

---

*End of Data Sourcing Plan v1 (2026-05-12)*
