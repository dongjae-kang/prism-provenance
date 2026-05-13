# PRISM Hero Design Journey (2026-05-13)

친구 [[김현미]]의 v5 Liquid Glass 시안을 base로 hero 디자인을 ~5시간 iterate한 결과물 + design exploration artifacts.

## 최종 디자인 ✨

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
