# Korean Political Discourse Provenance Layer — PRD v1.0

**Canonical spec.** Historical draft archived at `archive/PLAN_v0_superseded.md` (team-based design, superseded by solo + AI automation decision on 2026-04-19).

**Status**: Design locked (2026-04-19)
**Implementation start**: 2026-05-04
**First application**: 2026-06 지방선거

---

## 1. Mission Statement

### Korean (canonical)

한국의 정치 정보 환경은 구조적으로 극단화되어 있다. 지역 정서와 정당 충성도가 논리적 판단보다 우선하고, 같은 사실을 두고도 해석과 수용이 당파적으로 갈라진다. 이 구조 위에서 YouTube 는 한국 정치 담론의 가장 큰 시장이 되었고, 그 안에서 왜곡된 주장·출처 불명의 루머·클릭베이트가 빠르게 확산되고 있다. 그러나 어떤 콘텐츠가 어디에서 시작되어 어떻게 퍼지고 어떤 반응을 만드는지 체계적으로 확인할 수 있는 공공 인프라는 존재하지 않는다. 2026년 6월 지방선거는 이 공백이 실제 투표 행동에 영향을 미치는 고위험 구간이다. 이 프로젝트는 정치 YouTube 생태계를 (1) 원본 추적, (2) 파생·확산 패턴, (3) 수용자 반응 구조로 매핑하고, knowledge graph 로 정형화하여 누구나 탐색할 수 있는 공개 대시보드로 제공함으로써 misinformation 문제에 실질적으로 개입한다. 사용자는 특정 사건·주장·정치인에 대해 "지금 어떤 담론 환경에 놓여 있는가" 를 직접 확인할 수 있다. 이 작업은 개별 사실을 debunking 하는 차원을 넘어, 정치 정보가 확산되는 구조 자체를 드러냄으로써 pre-bunking 역할을 동시에 수행한다. 코로나 시기의 감염 지도가 복잡한 통계를 단순한 시각화로 바꿔 공공적 유용성을 만들어냈듯, 이 프로젝트는 불투명한 정치 정보 생태계를 한눈에 탐색 가능한 공공 자산으로 전환하는 것을 단기 목표로 삼는다. 장기적으로는 political misinformation provenance × knowledge graph ontology 교차 연구 영역에 경험적 방법론과 공개 데이터를 남기는 기여를 지향한다.

### English (canonical)

South Korea's political information environment is structurally polarized. Regional sentiment and partisan loyalty often take precedence over analytical judgment, and the same facts are absorbed and interpreted along party lines. Against this backdrop, YouTube has become the dominant arena for Korean political discourse, and within it distorted claims, rumors of unverified origin, and clickbait content spread rapidly. Yet no public infrastructure exists to systematically trace where a given piece of content originates, how it propagates, and what responses it generates. The June 2026 local elections mark a high-stakes window in which this absence may translate directly into voting behavior.

This project maps the political YouTube ecosystem along three axes: (1) provenance of original claims, (2) patterns of derivation and spread, and (3) the structure of audience response. The results are formalized as a knowledge graph and delivered through an open, queryable public dashboard, offering a concrete intervention into the misinformation problem. For any given event, claim, or political figure, users can directly examine the discursive environment it currently inhabits.

This is more than fact-checking done faster. By making visible the structures through which political information propagates, the project performs pre-bunking alongside debunking. Just as the COVID-era infection maps transformed complex statistics into simple, widely used public visualizations, this project aims to transform an opaque political information ecosystem into an explorable public asset. In the longer term, it seeks to contribute empirical methodology and open data to the research area where political misinformation provenance and knowledge graph ontology intersect.

---

## 2. Problem Statement

| 축 | 현황 |
|---|---|
| **구조적 토양** | 한국 정치의 극단화된 정보 환경. 당파적 수용 패턴 |
| **정보 유통 시장** | YouTube 가 정치 담론의 가장 큰 시장 |
| **오염 현상** | 왜곡된 주장·출처 불명 루머·클릭베이트의 빠른 확산 |
| **공백** | 원본 추적·확산 패턴·반응 구조를 체계적으로 보여주는 공공 인프라 부재 |
| **위험 구간** | 2026-06 지방선거 — 공백이 투표 행동에 직접 영향 |

## 3. Target Users

1. **1차: 유권자** — 공개 대시보드로 담론 환경 직접 확인
2. **2차: 언론·연구자** — 검증된 데이터·인용 원천
3. **3차: 팩트체커** — post-launch organic engagement (의도적으로 pre-launch 파트너십 없음)

## 4. Success Metrics

**Product**:
- 대시보드 주간 방문자 수, 재방문율
- 언론 인용 건수
- Top-N 항목당 평균 조회 시간

**Technical**:
- Claim-video matching: LLM-human agreement ≥ 95% (recursive gate)
- 파이프라인 주 2회 정기 업데이트 유지
- Precision ≥ 설정 threshold (recall 보다 우선)

**Ethical**:
- 편향 분포 메타데이터 투명하게 동반 (강제 대칭 없음)
- 모든 판단에 provenance 기록 (PROV-O)

**Academic (long-term)**:
- KG ontology 공개
- 방법론 논문 발표 가능성

---

## 5. Scope

### In Scope (MVP)

- **플랫폼**: YouTube 단일 (분석 축)
- **딜리버리**: 공개 대시보드 1개
- **뷰**: 주간 Top-N 이벤트 (옵션 B)
- **Canonical sources**: 7종 (아래 F2)
- **편향**: 투명성 4원칙

### Out of Scope (현재)

- ASR / 비디오 콘텐츠 분석
- 자동 claim verification (LLM 단독 배포)
- 카톡봇, 브라우저 확장 (post-MVP)
- 정치인 중심 뷰 (C 단계 확장)
- TV 토론회 영상, 외신, Substack·개인 블로그
- 팩트체커 공식 파트너십
- 학술 논문 집필 (장기 연기)

---

## 6. Features

### F1 — YouTube 데이터 파이프라인

**Discovery (Cascade)**:
```
[A. 시드 채널 40-60] + [D. 뉴스 앵커 Naver·Daum·Nate 주간 Top]
              ↓
     [B. YouTube 트렌딩 정치 카테고리로 증폭 대조]
              ↓
     [C. 정치인 이름 쿼리로 누락 재확인]
              ↓
         확정 후보 pool
```

**수집 신호 (22개)**:
- API 직접: 제목, 설명, 태그, duration, 조회수, 업로더, 업로드 시각, 채널 메타, Community tab posts, 댓글 (top + newest + pinned), topicCategories, 트렌딩 진입 여부, 자막/CC (업로더 설정 시)
- 파싱: Description 내 URL·chapters
- AI 파생: 썸네일 OCR, 썸네일 이미지 패턴, 감성, 클릭베이트 탐지, 댓글 봇 탐지 (계정 생성일·활동·중복)

### F2 — Canonical Claim Archive

**Sources (7종)**:
1. 정치인 공식 채널 (성명·보도자료)
2. 정당 공식 계정·보도자료
3. 주요 언론 직접인용 기사 (조선·중앙·동아·한겨레·경향 + 연합·뉴시스 wire)
4. 정치인·정당 공식 SNS (X, Facebook, Instagram — verified)
5. 네이버 블로그 (정치인 운영)
6. 국회 속기록 (접근 가능 범위)
7. 정부·대통령실·부처 공식 발표

**Out of scope**: TV 토론회 영상, 외신, Substack·개인 블로그

**Schema.org ClaimReview 호환** — 팩트체크 생태계 호환성 확보

### F3 — Claim Matching Engine

**Recursive Gate Framework**:
```
Gold set (50-100 pair 수동 라벨)
    ↓
LLM calibration (같은 set 에 실행)
    ↓
Agreement ≥ 95%? 
    Yes → 스케일 위임 + 5% spot-check
    No  → prompt iteration → 재실행
    ↓
주기적 gold set 갱신 → 재calibration
```

**Precision 우선** (법적 리스크 + 프로젝트 신뢰성)

**Match labels (3유형)**:
- Direct quote (원본 직접 인용)
- Partial quote + interpretation (일부 인용 + 해석·확장)
- Independent assertion (원본 링크 없는 유사 주장)

**Match record schema (provenance 필수)**:
```
{
  video_id, claim_id, match_type,
  confidence: 0-1,
  matched_by: "llm-vX + prompt-vY" | "human-reviewer-id",
  matched_at: timestamp,
  agreement_with_gold: true/false/null
}
```

**실패 방지 5장치**:
1. Canonical claim 텍스트의 구체성 (vague 배제)
2. Entity-based pre-filtering
3. 3유형 label 고정 (애매 판정 ↓)
4. Few-shot 예시를 gold set 에서 추출
5. 초기 스코프 좁히기 (특정 이벤트·발언 먼저)

### F4 — Knowledge Graph

**v0.1 Starter Framework** (담당자 합류 시 구체화):

**Node types (12)**: Claim, Source, Video, Channel, Politician, Event, CommentCluster, Comment, CommentAuthor, Topic, RhetoricalPattern, Ideology

**Edge types (14)**: originates_from, makes_claim, amplifies, contradicts, mentions, responds_to, cites, uploaded_by, in_cluster, authored_by, aligned_with, belongs_to, exhibits, about

**3 구조적 설계**:
- Probabilistic edges (confidence 점수 동반)
- PROV-O edge provenance (`created_by / created_at / evidence_pointer`)
- Temporal validity (사실·엣지에 유효 기간)

**표준 통합**:
- schema.org ClaimReview (Claim 노드)
- PROV-O (edge metadata)
- SIOC (Comment 관련)
- KR-WordNet + 선관위·국회 공개 DB (한국어 엔티티 grounding)

### F5 — Public Dashboard (MVP = B view)

**주간 Top-N 이벤트 뷰**:
- Top-N 토픽 (claim/issue 단위 — empirical 결정)
- 각 항목: 대표 영상 5개, 확산 곡선, 원본 추적, 댓글 클러스터, 편향 분포

**편향 투명성 4원칙 구현**:
1. 분포 메타데이터 ("이번 주: 보수 origin X% / 진보 origin Y%")
2. 병렬 sub-view (전체 + 보수 + 진보)
3. 방법론 공개 페이지
4. Less-represented side highlight

**확장 경로 (post-MVP)**: 정치인 drill-down 뷰 (C hybrid)

---

## 7. Technical Architecture (high-level)

```
[YouTube Data API v3 + yt-dlp]
        ↓
[Discovery Cascade (A/D/B/C)]
        ↓
[22-signal Collection + AI derivation]
        ↓
[Canonical Claim Archive (7 sources)]
        ↓
[Claim Matching Engine (recursive gate + LLM)]
        ↓
[Knowledge Graph (probabilistic + PROV-O + temporal)]
        ↓
[Public Dashboard (B view, 편향 4원칙)]
```

**실행 주체**: Codex-driven Python pipeline (Jack 관리, AI 자동화 중심)

**세부 기술 선택 (담당자 위임)**: KG DB (Neo4j vs lightweight), embedding 모델 (KoSimCSE vs 다국어), LLM, frontend stack

---

## 8. Non-Functional Requirements

**Legal**: 공직선거법 82조의4 (허위사실공표), 250조 (후보자 비방), 2024 AI 딥페이크 조항 compliance. 한국 내 변호사 자문 필수.

**Ethical (13 설계 원칙 + 투명성 원칙)**:
- 강제 대칭 금지, 투명 분포
- Partisan-correcting curation 금지
- 모든 판단에 provenance

**Reproducibility**: Gold set + 프롬프트 버전 + 매칭 로그 공개 가능한 구조

**Performance**: 주 2회 파이프라인 업데이트, 대시보드 로드 < 3초

---

## 9. Timeline

| 시점 | 이벤트 |
|---|---|
| 2026-04-19 | 설계 완료 (이 세션) |
| 2026-04-20 ~ 05-03 | 학업 마감 시즌, 섭외 진행 |
| **2026-05-04** | 구현 시작 |
| 2026-05 전반 | Gold set 구축, 파이프라인 구축, recursive gate 첫 calibration |
| 2026-05 후반 | 대시보드 alpha, 팀 내 beta 테스트 |
| 2026-05-30 ~ 06-01 | Public launch (선거 3-5일 전) |
| **2026-06-03** | 지방선거 당일 — 트래픽·사용 패턴 데이터 수집 |
| 2026-06+ | 학술 정리 착수 (장기) |

---

## 10. Team

| 역할 | 담당 | 기대 |
|---|---|---|
| **Lead + CS/Data/KG** | Jack | 전체 설계·구현 주도 |
| **Codex (AI)** | 기술 실행 | 코딩·데이터 파이프라인 |
| **Claude Code (AI)** | 설계 메타 | 플래닝·메모리·의사결정 보조 |
| **도메인 감수자** | 한국인 리크루트 | 한국 정치 맥락·편향 투명성 구현 감수 |
| **Writer/Comms** | 한국인 리크루트 | 논문·advisor outreach·영어 글쓰기 |
| **Data Reviewer** | 한국인 리크루트 | Gold set 초기 큐레이션 |
| **법률 자문** | 한국 현지 변호사 | 공직선거법·딥페이크 조항 검토 |

리크루트는 설계 완성 후 진행. 자세한 역할·기대는 `memory/team_recruitment_prep.md` 참조.

---

## 11. Open / Deferred Decisions

상세 로그는 `memory/project_korean-provenance-layer.md` 의 **Deferred Decisions Log** 섹션 참조.

**카테고리**:
- Top-N Framework: claim vs issue 단위, "정치 콘텐츠" 정의, seed 채널 구체 리스트, 영향력 가중치
- KG Ontology: 노드·엣지 구체 semantics, confidence 체계, 한국 특화 엔티티
- Canonical Source: 소스 위계 기록, unverifiable 기준, 삭제 콘텐츠 처리, SNS 수집 깊이, 네이버 블로그 크롤링
- Dashboard: 4원칙 UI 구체 구현
- Legal: Jack 이 한국 변호사 섭외 트랙

**결정 주체 원칙**: 담당자 합류 시 그 사람의 rationale 로 구체화 (Principle #11 Design-to-attract).

---

## 12. Risks & Mitigations

| ID | 리스크 | 완화 |
|---|---|---|
| R1 | Claim matching 70% gate 실패 | 실패 방지 5장치 + 실패 시 스코프 축소 (Plan B) |
| R2 | 법적 이슈 | W1-2 내 한국 변호사 preliminary check |
| R3 | 한국어 엔티티 추출 품질 | KR-WordNet + 선관위·국회 DB grounding |
| R4 | 편향 분포 극단적 비대칭 | 투명성 4원칙 (분포 공개 + less-represented highlight) |
| R5 | YouTube API quota 한계 | 기본 10K units/day 시작, 부족 시 상향 or yt-dlp 병용 |
| R6 | Jack 단독 실행 capacity 초과 | AI 자동화 비중 극대화, Codex 활용 |

---

## 13. Design Principles (이 프로젝트 전반에 적용)

13 메타 원칙 상세: `memory/feedback_project_design_principles.md`

요약:
1. Design-first, resources-later
2. No padding
3. Concrete grounding over rhetorical punch
4. Active intervention verb
5. No tribal language
6. Surgical edit over rewrite
7. Internal substance > external surface
8. Fact-check domain context
9. Role autonomy respect
10. Simple-useful > complex-ambitious
11. Design-to-attract, not design-to-dictate
12. 판단의 근거를 데이터 레벨에 구조화
13. Defer with discipline, not drift

---

## 14. Canonical References

- `PLAN.md` — 초기 드래프트 (historical, superseded by this PRD)
- `memory/project_korean-provenance-layer.md` — 세부 결정 로그 + Deferred Decisions
- `memory/session_2026-04-19_initial-design.md` — 설계 세션 티키타카
- `memory/feedback_*.md` — 13 원칙 + 미션 원칙 + 투명성 원칙
- `memory/team_recruitment_prep.md` — 리크루트 섭외 가이드
