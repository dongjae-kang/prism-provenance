# Korean Political Discourse Provenance Layer — Track 1.5 (2026-06 지방선거)

## Context

한국은 2026-06 지방선거를 앞두고 있고, misinformation 확산 속도가 정보 환경 오염을 가속하고 있다. 기존 팩트체커들은 사후 디벙킹에 머물러 있고, 유권자가 콘텐츠를 접하는 **point-of-exposure**에서의 개입이 비어 있다.

**목표**: 2026 지방선거 기간 매주 **Top-N 바이럴 정치 클레임**을 선정해, 그 클레임의 **원본(canonical source) 추적** + **YouTube 상 파생·유통 패턴 매핑**을 유권자 접점(브라우저 확장 + 카톡봇)에 실시간 surface하는 도구 출시. Jack의 "PRISM" 장기 관심의 **symbolic MVP**이자 Track 2 (2027-03 대선)의 design feedback 공급원.

**핵심 설계 원칙 (Jack의 판단에서 도출)**:
- **이벤트/클레임 기반** (정치인 기반 아님) — 전국 스코프 자연 달성, 편향 신호 최소화
- **텍스트 only 파이프라인** — ASR/비디오 분석 제거, YouTube 메타데이터(제목/설명/태그/썸네일 OCR) + 댓글 + 조회수 활용
- **Canonical source는 텍스트 원본** — 정치인 공식 성명·보도자료, 주요 언론 직접인용 기사, 정치인 공식 Twitter/X
- **Human-in-the-loop 필수** — LLM 자동 배포 금지, 명예훼손/hallucination 방어
- **Track 2로 이월**: 완전 자동화 매칭, ASR/영상 분석, multi-hop graph 2차+ derivation

## Recommended Approach

### Scope (hard limits)
- **커버리지**: 주간 **Top 10-20 바이럴 정치 클레임** (전국, 양 진영 대칭 강제)
- **플랫폼**: YouTube 1개 (메타데이터 + 댓글)
- **Canonical source**: 텍스트 3종 (공식 성명, 언론 인용 기사, 공식 Twitter)
- **업데이트 주기**: 주 2회 batch
- **스코프 보호 메트릭**: 양 진영 클레임 비율 ±10% 이내

### 5개 컴포넌트

**C1. Canonical Claim Archive** — 텍스트 기반 원본 DB
- 3종 소스에서 클레임 수집: 정치인 공식 채널, 주요 언론 기사, 공식 SNS
- 스키마: claim_id, claimant, date, original_text, source_url, category(학력/경력/발언/정책/관계)
- **Unverifiable flag**: 원본 텍스트 부재 시 "근거 텍스트 없음" 명시 라벨

**C2. YouTube Spread Tracker** — 파생 유통 매핑 (ASR 없음)
- YouTube Data API v3로 정치 관련 Top-N 바이럴 영상 수집 (주간)
- 영상당 수집: 제목, 설명, 태그, 썸네일(OCR로 텍스트 추출), 조회수, 업로더, 업로드 시각, Top-100 댓글
- 각 영상 → Canonical Claim DB의 어느 claim 언급? LLM 보조 매칭 + **human 검증 전 배포 금지**
- 변형 라벨 (객관 기준만): "원본 직접 인용" / "일부 인용 + 해석 추가" / "원본 링크 없음 - 독립 주장"

**C3. Comment Spread Analysis** — 여론 확산 레이어
- 댓글 텍스트 클러스터링 (주제별 그룹)
- 댓글 bot detection: 계정 생성일, 활동 패턴, 중복 텍스트 기반 간단 휴리스틱
- **Raw 데이터 + 필터링 데이터 양쪽 보존** — 투명성
- 댓글별 태그: "원본 주장 반복" / "확장 주장" / "반박" / "off-topic"

**C4. Knowledge Graph** — 통합 구조
- 노드: canonical_claim, youtube_video, comment_cluster, politician, event
- 엣지: derives_from, mentions, contradicts, amplifies, responds_to
- W1-2 공식 활동: ontology 설계 + graph DB 스키마
- 시각화: 주간 "바이럴 클레임 지도" 공개 대시보드

**C5. Point-of-Exposure Surface** — 유권자 접점 도구
- **브라우저 확장**: YouTube 영상 재생 시 상단 배너로 "이 영상은 원본 X의 파생 - 변형 유형: Y" 표시
- **카톡봇**: 링크 붙여넣기 → 동일 provenance 정보 반환
- **공개 대시보드**: Knowledge Graph 시각화 + 주간 Top-N claim 아카이브
- 기반: Jack의 기존 Article Trust Layer + Social Media Trust Extension 발전

### Team & Governance
- **리드**: Jack (SIPA AI Research Lab 공식 프로젝트로 승격)
- **팀**: Lab 4명 + Jack = 5명
- **역할 분배** (kickoff에서 확정):
  - 1명: Canonical Claim Archive 큐레이션 (텍스트 수집·정리)
  - 1명: YouTube 데이터 수집 파이프라인 + 댓글 분석
  - 1명: Knowledge Graph / 백엔드
  - 1명: 브라우저 확장 + 카톡봇 + 대시보드
  - Jack: 총괄 + 편향 대칭 체크 + advisor 커뮤니케이션 + 법적 검토 리드

### External Inputs (Jack 부담 최소화)
- **SNU FactCheck 파트너십 강제 제거** (Jack 관계 없음) → 방법론 공개 + 오픈소스 데이터 전략으로 편향 방어 대체
- **Ibrahim 교수** — Digital Provenance/C2PA 자문 (기존 관계 활용)
- **Advisor 루프**: Camille François, Jennifer Jones — 피드백 only, 개입 없음
- 팩트체커 engagement는 **출시 후 organic**으로 (미리 부탁하지 않음)

## Timeline (45일, 2026-04-19 → 2026-06-03)

- **W1 (4/19-4/25)**: Lab kickoff + 스코프 합의 + Knowledge Graph ontology 설계 + YouTube API quota 확보 + **Technical spike**: LLM 기반 claim-to-video 매칭 정확도 테스트 (샘플 20개)
  - **Go/no-go gate**: 매칭 정확도 ≥ 70% or human review 비용 감당 가능 여부
- **W2-3 (4/26-5/9)**: Canonical Claim Archive 초기 입력 (50-100개) + YouTube Spread Tracker 파이프라인 + 댓글 수집 · bot detection
- **W3-4 (5/10-5/16)**: Knowledge Graph 백엔드 + 브라우저 확장 alpha
- **W4-5 (5/17-5/23)**: 카톡봇 + 공개 대시보드 + alpha 테스트 (팀 내)
- **W5-6 (5/24-5/30)**: Beta 배포 + 20-30명 유저 피드백 (SIPA 한국인 + 국내 네트워크)
- **선거 3-5일 전 (5/30-6/1)**: Public launch + 네트워크/미디어 outreach
- **선거 당일 (6/3)**: 데이터 수집 (트래픽, 사용 패턴)
- **선거 후 1주**: Track 2 설계 feedback 정리

## Critical Files & Assets

코드베이스 신규 구축:
- `~/github/prism-provenance/` — 코드 repo
- `~/vault/projects/korean-provenance-layer.md` — 프로젝트 노트
- MEMORY: `project_korean-provenance-layer.md`

재활용 자산:
- **Article Trust Layer** (Jack 로컬 데모) — C5 기반
- **Social Media Trust Extension** (Jack 로컬 데모) — 브라우저 확장 기반
- `~/vault/people/Ibrahim.md` — 자문 연결
- `~/vault/people/Camille François.md` — Advisor (2026-04-10 메일 발송 완료)
- `~/vault/projects/sipa-ai-research-lab.md` — Lab 승격 문서화

## Verification

E2E 검증:
1. **Canonical Archive 품질**: 50-100 claim 입력, 100% 출처 텍스트 링크 유효
2. **매칭 정확도**: 랜덤 50개 영상-claim 매칭을 Jack + 팀원 2명이 독립 검증. **팀 합의율 ≥ 80%** 목표 (팩트체커 외부 검증은 post-launch)
3. **Bot detection**: 댓글 샘플 500개 대상. 휴리스틱으로 걸러낸 bot 의심 계정 정밀도 70%+ (Jack 수동 스팟체크)
4. **도구 작동**: 브라우저 확장 10개 영상 시나리오, 카톡봇 100 링크 파싱 테스트, 대시보드 로드 시간 < 3초
5. **Beta 피드백**: 20-30명 유저 (NPS + 정성 피드백)
6. **편향 대칭**: 양 진영 claim 비율 **±10% 이내** 주간 체크. Jack 직접 검토.
7. **법적 체크**: 공직선거법 82조의4, 250조 + 2024 AI 딥페이크 조항 해당 여부 **W1-2 내** 선제 검토. Columbia Law 선배/SIPA 한국 동문 변호사 자문.

## Out of Scope (Track 2 예약)

- Full multi-hop provenance graph (2차+ derivation 자동 추적)
- ASR / 비디오 콘텐츠 분석
- 자동 claim verification (LLM 단독 배포)
- 플랫폼 확장 (Twitter, TikTok, 카톡 채널)
- 학술 논문 집필 (CHI/CSCW/FAccT)
- 정책 백서 (SIPA Tech Policy)
- 정치인 staff active engagement
- 공식 팩트체커 파트너십

## Critical Open Items (W1 내 해결)

1. **Lab kickoff 합의**: 4명 팀원이 이 스코프에 동의하는지. 역할 분배 확정.
2. **Technical spike 실행**: LLM 매칭 정확도 20개 샘플 테스트. 결과로 scope 재조정.
3. **YouTube Data API quota 확보**: 기본 10,000 units/day로 시작. 부족 시 상향 신청.
4. **법적 preliminary check**: 공직선거법 해당 여부 + 2024 딥페이크 조항. 필수 사전 검토.
5. **Chrome Web Store 심사 타임라인 확인**: 배포 일정 역산.
6. **Knowledge Graph DB 선택**: Neo4j vs 가벼운 대안 (SQLite + 관계 테이블). W1에 결정.

## Risk Register (명시)

- **R1. LLM 매칭 정확도 부족**: W1 spike 결과 70% 미만이면 scope 재조정 (human curation 비율↑)
- **R2. YouTube API quota 한계**: Top-N 축소 or 상향 신청
- **R3. 댓글 bot 오염이 너무 심함**: 댓글 레이어 weight 낮추고 메타데이터 중심으로
- **R4. 법적 이슈 발견**: W1-2 법률 자문에서 red flag 시 즉시 scope 축소 or 출시 보류
- **R5. 팀 capacity 부족**: 5개 컴포넌트 중 우선순위 C1-C2-C5 (MVP core), C3-C4 후순위
- **R6. 편향 대칭 meter 유지 실패**: 주 1회 Jack 체크, 실패 시 해당 주 curation 재조정
