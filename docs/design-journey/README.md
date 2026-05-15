# PRISM Design Journey (2026-05-13 → 2026-05-14)

5/13 hero v8 + vector pipeline → 5/14 v9.2 final로 progressed. v9이 main deliverable, v8은 milestone reference로 보존.

## 최종 — v9.2 (2026-05-14)

**`docs/demo-v9.html`** — main demo (dev fetch 기반)
**`docs/demo-v9-standalone.html`** — single-file (data inline, 332KB) for friend hand-off

### 핵심 features

1. **양향자/추미애 balanced data** — 5/14 새벽 query `"양향자 추미애 경기지사"` max 20 → 16 video 선정 (양향자 15 + 추미애 14 + both 13). 5/13 자막 실측용 데이터 (추미애-only 8개)는 archive.
2. **MDS 2D scatter (cosine 거리 보존)** — 1024차원 Voyage embedding의 cosine distance를 sklearn MDS로 평면 투영. 점 사이 시각 거리 = 실제 의미 거리. visual ↔ algorithmic consistency 자연 확보.
3. **Vector arrows from centroid** — slim chevron + 2-stop gradient + drop shadow. SCALE 0.78로 위/아래 여백 균형.
4. **임의 색상 3개** — indigo / teal / rose. 정치 성향·등급과 무관 명시.
5. **"How" hover toggle** — dark button (ink-mid bg + white text), 헤더 우상단. Hover 시 작동 원리 fade in.
6. **카드 3열 항상** — 모바일에서도 가로 정렬 (vertical stacking 회피). compact height ~92-110px.
7. **Plot frame 자체가 main hero** — 흰색 card wrapper 폐기. 황토색 (#fafaf7) frame이 paper-2 위에 직접.

### 빌드 chain

```
~/Desktop/prism-pipeline-0513/scripts/
  01b_yang_chu_balanced.py    # 신규 search query
  06_sample_fill.py            # Haiku vision + 자막 + Voyage embedding
  07_max_disperse.py           # max-disperse 3 selection
  08_comments_fetch.py         # commentThreads.list per video
  09_mds_projection.py         # 신규 — MDS 2D
  build_standalone_v9.py       # JSON inline + embedding strip
```

### 학습 patterns (이번 iteration 산출)

- **데이터 완전성 함정** — "real data 다 넣자" → 카드 무너짐. 정보 선별이 디자인의 일부.
- **MDS visualization for high-dim cosine** — algorithmic 결과 (1024-dim cosine)와 시각 (2D 평면)이 mismatch될 때 MDS로 projection하면 자연 정합.
- **Visual-algorithmic consistency** — 사용자가 보는 공간에서 알고리즘도 측정해야 동의 가능. (Cosine 유지 + MDS 투영이 그 답.)
- **Discovery query 재활용 self-validation 필수** — 자막 실측용 query를 demo로 surface하면 자기 위반 가능. candidate balance check.
- **격리 → 검증 → 통합 워크플로우** — `mds-test.html`로 SVG MDS 디자인 격리 iteration (10+ Jack rounds) → 완성 후 v9 React port. 큰 React 코드베이스에서 design polish 안전.
- **음성 인식 정정** — Jack 발화 "보도 시각" → 텍스트 "세계관"으로 잘못 변환되는 패턴 감지 + 즉시 정정.

## 5/13 milestone — v8 hero (preserved)

**`docs/demo-v8.html`** — Hero + Today 탭 final (2D web Image #4 ceiling 도달)

핵심 요소:
- **Liquid Glass design tokens** (현미 v5 base + ambient prism light)
- **Hero**: origin (canonical glass cube, spatial depth perspective) + 5 outlet (균일 size 170×130, isometric perspective default)
- **Hover**: card 1.25× lift + full-card info overlay (channel/title/spec/view/time)
- **Edge 3-axis virality cone**:
  - Cone wide 20%~100% (view 비례)
  - Subtle spec color saturation (mix 5~30%)
  - Drop-shadow halo intensity (4~22px)
- **Mini legend in header** (4 spec color + 굵기/흐름 의미)
- **Today 탭**: PEEK params 강화된 3D Deck swipe + active card drop-shadow halo

## Design Exploration (working artifacts)

### `prism-layout-sketches.html` — 4 layout 옵션 비교
Design thinking phase에서 도출된 4가지 visual language 후보:
- **A**: Layered Glass Constellation (origin 중심 + 데이터 비례 scale)
- **B**: Depth Stack (origin 뒤로 blur, outlet 앞으로 sharp)
- **C**: Outlet 자체가 spec color glass cube
- **D**: Vertical Light Streams (친구 SpectrumHero 응용)

최종 선택: A + C + light B hybrid.

### `prism-3d-poc.html` — Three.js 3D 글래스 cube POC
WebGL PBR rendering으로 Image #4 photoreal 글래스 큐브 reproduce 시도:
- 6 truncated 4-sided prism crystals
- Glass material (transmission 0.95+, ior 1.52, clearcoat)
- HDR equirectangular env map (sunrise hemisphere)
- Edge ShaderMaterial moving wave + light packet

학습: web 2D ceiling 7.5/10, 3D ceiling 9.5/10. 다만 3D 통합 risk + 자정 새벽 누적 시도로 fundamental 한계 도달, 2D로 최종 회귀.

### `prism-terrain-poc.html` — Image #13 spectrum terrain POC
Data-driven 3D heightmap (Gaussian peak at outlet positions, spec color vertex colors, wireframe+points overlay):
- 5 outlets + origin → Gaussian terrain peaks
- Vertex colors = spec color Gaussian-weighted blend
- Wireframe + Points overlay = Image #13 dotted aesthetic

학습: PRISM 본질 (분광)에 직접 align되지만 YouTube 데이터 연결 약함. Crystal node 방향과 fundamentally 다른 visual language.

## Design Lessons

이번 iteration에서 도출된 핵심 학습:

1. **Reference image 본질 분석 매번 cycle 시작에**: Image #4 (crystal photo) vs Image #13 (terrain) — fundamentally 다른 visual language. 직역 위험.
2. **Incremental edit ≠ quality 누적**: 표면 add (sphere halo, octahedron fragment, dual cylinder) = cheesy cluttered. 본질 변환만 quality.
3. **Color discipline**: 무지개 형형색색 = 초등학생 정서. Single accent + saturation control이 pro.
4. **2D ceiling 7.5/10 vs 3D ceiling 9.5/10**: 2D + CSS perspective + 명확한 narrative가 안전한 ceiling.
5. **Hover overlay = scaled size 계산해서 fit 설계**: 단순 add 대신 [base × scale] = scaled size 안에 정보 정확 배치.
6. **Cone/ribbon edge — 분광 narrative 시각화**: 단순 line이 아닌 3-stage narrative (origin → frosted → outlet spec burst).

## 다음 step

- 친구 김현미: v5 시안에서 어떤 부분이 잘 살았고 어떤 부분이 더 손봐야 할지 피드백
- 친구 재훈+샘님: backend integration 관점에서 review
- 친구 아인: governance/PRD perspective review
- Production integration → 5/16~17 wrap-up → 5/30 launch

## How to view

PRIVATE repo이므로 GitHub Pages 자동 활성 X. 두 가지 옵션:

**Option 1 — htmlpreview.github.io proxy (즉시 가능)**:
- 최종 hero: `https://htmlpreview.github.io/?https://github.com/dongjae-kang/-PRISM/blob/main/docs/demo-v8.html`
- Layout sketches: `https://htmlpreview.github.io/?https://github.com/dongjae-kang/-PRISM/blob/main/docs/design-journey/prism-layout-sketches.html`
- 3D POC: `https://htmlpreview.github.io/?https://github.com/dongjae-kang/-PRISM/blob/main/docs/design-journey/prism-3d-poc.html`
- Terrain POC: `https://htmlpreview.github.io/?https://github.com/dongjae-kang/-PRISM/blob/main/docs/design-journey/prism-terrain-poc.html`

**Option 2 — Local clone**:
```bash
git clone https://github.com/dongjae-kang/-PRISM.git
cd -PRISM/docs
open demo-v8.html
```

⚠️ 노트북에서 보는 게 제일 좋습니다. 카드 hover 효과는 마우스 필요. 모바일은 hover 없음.
