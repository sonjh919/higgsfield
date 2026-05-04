---
name: higgsfield-fx
description: 정지 이미지용 Higgsfield 프롬프트의 FX(시각효과) 레이어를 보강하는 스킬. Impact·Particle·Fluid·Optical·Atmospheric·Magical·Material 7카테고리의 영문 어휘와 구절을 제공. 사용자가 "FX 추가", "이펙트 보강", "타격감 / 파편 / 입자 / 안개 / glow / lens flare / 잔상 추가", "/higgsfield-fx" 등을 말하면 이 스킬을 사용한다. 영상 FX는 본 스킬의 범위 밖이다.
user-invocable: true
---

# Higgsfield FX Layer Builder (정지 이미지 전용)

Higgsfield 정지 이미지 프롬프트의 **FX(시각효과) 레이어**만 좁게 담당하는 add-on 스킬. 본 프롬프트(MCSLA / 9요소 골격)는 다른 Higgsfield 프롬프트 워크플로우가 만들고, 이 스킬은 그 결과물에 FX 어구를 자연스럽게 얹거나 단독 FX 구절을 생산한다.

- **다루는 것**: 7카테고리(Impact / Particle / Fluid / Optical / Atmospheric / Magical / Material)의 영문 어휘·구절·결합 룰
- **다루지 않는 것**: 영상 FX, 비포토리얼 스타일 변환, 본 프롬프트의 Subject·Camera·Action 골격

## 입력 모드 두 가지

| 모드 | 입력 | 출력 |
| --- | --- | --- |
| **augment** | 이미 작성된 영문 프롬프트 + (선택) 강조 카테고리 | FX 어구가 삽입된 보강된 프롬프트 1개 |
| **standalone** | 씬 컨텍스트(자연어) + 강조 카테고리 | 본 프롬프트에 합칠 영문 FX 구절 (카테고리별 1~3어구) |

**사진은 두 모드 모두의 보조 입력**으로 결합 가능 (`augment+image` / `standalone+image`). 사진이 첨부되면 `Read` 도구로 멀티모달 분석하여 톤·재질·기존 시각 요소·광원/색감을 추출하고, 사용자 자연어 설명과 결합한다.

판정 룰: 입력에 `Subject:` / `shot on` / `photorealistic` 같은 영문 프롬프트 흔적이 있으면 augment, 그 외 자연어 묘사면 standalone. 애매하면 한 줄 질문.

## 절차 (4단계)

### 1단계: 모드 판정 + 사진 체크
- 입력의 영문 프롬프트 흔적을 본다 → augment / standalone.
- **사진 첨부 여부 확인** — 첨부됐으면 `Read` 도구로 이미지 파일을 읽어 멀티모달로 분석한다 (`Bash`로 `cat` 금지). 다음을 추출:
  - **이미 보이는 시각 요소** — 모닥불 불빛, 먼지, 안개, 입김 등 사진에 이미 렌더링된 FX 후보 (중복 방지용 목록)
  - **재질** — 갑옷·의상·바닥·벽·무기 등 (Material 카테고리 매핑 후보)
  - **광원·색감** — 키 라이트 방향과 강도, 지배 색조 (Color anchor 고정)
  - **톤** — 다크판타지 / sci-fi / horror 등 (2단계 자동 판정)

### 2단계: 씬 톤 추출
다음 중 분류 (FX 어휘 선택의 기준):
- **dark fantasy** — 중세·언데드·마법 액션
- **sci-fi** — 우주·미래·메카·에너지 무기
- **horror** — 어둠·불안·초자연
- **action** — 추격·격투·총격
- **everyday / romance** — 일상·로맨스·드라마
- **landscape** — 풍경·자연

**사진이 있으면 사진의 톤이 우선**한다. 사용자 자연어 설명이 사진 톤과 충돌하면 (예: 사진은 일상 카페인데 설명은 sci-fi 전투) 한 줄 질문으로 어느 쪽을 따를지 확인.

### 3단계: 카테고리별 어구 조립
- 사용자가 강조한 카테고리를 **primary 1개**로 잡고, 톤에 어울리는 **secondary 1~2개**만 추가 (총 2~3개)
- **사진에 이미 보이는 FX는 중복하지 말고 보강·구체화만** — 예: 사진에 모닥불 불빛이 이미 있으면 "warm firelight" 추가 ❌, "warm firelight rim-lighting the helm crest" ⭕ (위치·방향·강도를 구체화)
- 4개 이상 동시 강조 요청은 "고밀도 충돌 경고" 룰 발동 → 사용자에게 좁혀달라고 한 줄 질문

### 4단계: 출력
- **augment**: 본 프롬프트에 콤마로 삽입. **삽입 위치**는 `lighting` 다음, `color palette` 앞 (또는 `mood` 직전). 단어수 60 초과 시 사용자에게 단축 여부 확인.
- **standalone**: 카테고리 라벨 + 영문 FX 구절을 코드 블록 1개로.

---

## 7개 FX 카테고리

각 카테고리의 영문 어휘는 그대로 프롬프트에 들어가는 **즉시 사용 가능한 어구** 형태로 정리.

### 1. Impact — 충돌·타격 순간

**정의**: 두 물체가 만나는 그 찰나의 시각화. 무기, 주먹, 폭발, 발사체.

**핵심 동사·명사**:
`sparks fly` · `sparks scatter` · `dust burst` · `shock ring` · `concussion ripple` · `recoil wave` · `fragment trajectory` · `splinters spray` · `metal scoring` · `pulverized` · `mid-strike` · `point of contact` · `shrapnel arc`

**클리셰 구절 (톤별)**:
- (dark fantasy) `the axe blade catches the spine, vertebrae shattering outward, bone shards suspended mid-air`
- (action) `fist connects, sweat and dust spraying off in a tight cone`
- (sci-fi) `plasma round impacts the visor, sparks tracing a fan from the entry wound`
- (horror) `the door splinters inward, jagged wood fragments hanging in the doorway`
- (action) `bullet tears through the wall, dust burst billowing toward camera`

**밀도 룰**: Impact는 **단일 시점**으로 고정. "during the swing" + "after the impact" 두 시점을 한 컷에 담지 말 것 → 모션 모핑 유발.

**Reference presets** (when converting to video): `Explosion` / `Building Explosion` / `Atomic` / `Plasma Explosion`.

---

### 2. Particle — 입자 효과

**정의**: 공기 중에 떠 있거나 느리게 흩날리는 작은 시각 요소.

**핵심 동사·명사**:
`embers floating` · `ash drifting` · `dust motes suspended` · `microdebris` · `pollen swirling` · `snow crystals tumbling` · `glitter cascading` · `paper shreds` · `feathers settling` · `bone dust` · `glass fragments mid-air`

**클리셰 구절**:
- (dark fantasy) `warm embers floating up from off-camera braziers, cooling into ash near the upper frame`
- (sci-fi) `microdebris drifting in zero gravity, catching cool blue rim light`
- (winter / landscape) `fine snow crystals tumbling diagonally, backlit into bright pinpricks`
- (everyday) `dust motes suspended in a single shaft of afternoon light through a window`
- (horror) `pale ash falling like soft snow over the empty courtyard`

**밀도 룰**: Particle은 배경 요소이므로 **장면 전체에 한 종류만**. embers + ash + snow를 한 컷에 섞으면 시각적 노이즈가 됨. 한 톤만 골라서 양과 방향만 명시.

**Reference presets** (when converting to video): `Disintegration` / `Clone Explosion`.

---

### 3. Fluid — 액체·기체 거동

**정의**: 흐르거나 흩어지거나 피어오르는 유체. 연기·증기·물·피·입김·불.

**핵심 동사·명사**:
`steam rises` · `smoke curls` · `breath fog` · `blood mist` · `water spray` · `oil drips` · `flames lick` · `vapor coiling` · `mist clinging to the ground` · `droplets suspended` · `tears welling` · `sweat beading`

**클리셰 구절**:
- (dark fantasy) `the warrior's breath fogging in the cold night, twin plumes from the helm slits`
- (everyday) `steam rises in slow curls from a ceramic mug, catching warm window light`
- (action) `tire smoke curling around the chassis, gray-white tendrils trailing the spin`
- (horror) `pale mist clinging to the floor, knee-high, refusing to dissipate`
- (sci-fi) `coolant venting in a horizontal jet, white vapor cutting across the frame`

**밀도 룰**: Fluid는 **방향**과 **속도감**을 항상 명시. "smoke" 단독 ❌ → "smoke curls upward, slow" ⭕. 방향 누락 시 모델이 평면적으로 그림.

**Reference presets** (when converting to video): `Water Bending` / `Splash Transition`.

---

### 4. Optical — 광학·렌즈 효과

**정의**: 카메라/렌즈가 빛을 처리하면서 생기는 효과. 조명원과 함께 작동.

**핵심 동사·명사**:
`lens flare` · `anamorphic streak` · `bloom around highlights` · `motion blur trail` · `bokeh balls` · `light shafts` · `god rays` · `chromatic aberration` · `subtle vignette` · `rack focus implication` · `streak of glare`

**클리셰 구절**:
- (sci-fi) `anamorphic blue lens flare cutting horizontally across the bridge`
- (action) `motion blur trail on the axe arc, smearing the blade into a silver crescent`
- (everyday) `soft golden bloom around the practical bulb, warm bokeh balls in the background`
- (horror) `chromatic aberration on the candle flame, red-cyan fringing at the edges`
- (landscape) `god rays piercing through canopy gaps, defining the volumetric air`

**밀도 룰**: Optical은 **광원과 페어링**해야 자연스러움. "lens flare"만 두지 말고 어떤 빛에서 오는지 짝지을 것 → "lens flare on the rising sun".

**Reference presets** (when converting to video): `Glow Trace` / `Innerlight`.

---

### 5. Atmospheric — 환경·대기

**정의**: 씬 전체에 깔리는 분위기 요소. 날씨·공기 상태·대규모 환경 입자.

**핵심 동사·명사**:
`fog rolling in` · `haze softening the distance` · `rain streaks` · `falling debris` · `heat haze rippling` · `volumetric light through fog` · `sandstorm wall` · `snow flurry` · `ash storm` · `dust haze` · `humidity bloom`

**클리셰 구절**:
- (dark fantasy) `low fog rolling between the broken curtain walls, ankle-deep, cooling the foreground`
- (horror) `dense fog reducing background contrast, silhouettes barely visible at twenty paces`
- (action) `rain streaking diagonally, headlight beams cutting hard cones through the downpour`
- (sci-fi) `heat haze rippling above the engine cowl, distorting the stars beyond`
- (landscape) `volumetric morning light pouring through pine canopy, dust haze defining each beam`

**밀도 룰**: Atmospheric은 **광원과 결합**될 때 가장 강함 (volumetric). 어두운 씬에는 fog/haze를, 밝은 씬에는 god rays/heat haze를. 섞으면 회색 죽 됨.

**Atmospheric lexicon**: `volumetric light` / `fog` / `mist` / `dust haze` / `god rays` — Higgsfield 플랫폼의 표준 환경 어휘.

---

### 6. Magical / Energetic — 마법·에너지

**정의**: 비현실적이지만 다크판타지·sci-fi에서 포토리얼하게 통합되는 빛·에너지.

**핵심 동사·명사**:
`glow emanates` · `runes pulsing` · `energy trail` · `residue mist` · `eye luminance` · `palm aura` · `arcane sparks` · `weapon imbued with light` · `faint inner light` · `crackling tendrils` · `phosphorescent`

**클리셰 구절**:
- (dark fantasy) `faint blue runes pulsing along the axe edge, casting cool light on the wielder's gauntlet`
- (sci-fi) `the reactor core emanating a steady amber glow, residue mist coiling above the vents`
- (horror) `the figure's eyes faintly luminous, sickly green, no other light source visible`
- (dark fantasy) `arcane sparks dancing along the chainmail, reacting to the strike`
- (sci-fi) `energy trail streaking from the fired weapon, ionized air glowing white-blue`

**밀도 룰**: Magical은 **광원 역할**도 하므로 환경 라이팅과 충돌 주의. 마법 빛이 키 라이트가 될지, 보조 라이트가 될지 명시.

**Reference presets** (when converting to video): `Luminous Gaze` / `Innerlight` / `Glow Trace`.

---

### 7. Material Reaction — 물질 반응

**정의**: 충돌·압력·온도에 대한 재질의 즉각적 반응. 갑옷·옷·바닥·유리·금속.

**핵심 동사·명사**:
`fabric flutter` · `chainmail rippling` · `cloth whipping in motion` · `metal scoring` · `glass crack pattern` · `ground impact crater` · `wood splintering` · `leather creaking` · `paint chipping` · `frost spreading` · `ice fracture lines`

**클리셰 구절**:
- (dark fantasy) `chainmail links rippling along the swing arc, individual rings catching firelight`
- (action) `windshield glass cracked into a star pattern at the bullet entry, fragments suspended`
- (dark fantasy) `cloak fabric whipping forward from the swing momentum, cloth folds frozen mid-flow`
- (sci-fi) `armor plate scored by laser fire, edges still glowing orange`
- (horror) `frost spreading visibly across the wooden floor from the figure's footsteps`

**밀도 룰**: Material reaction은 **씬의 핵심 재질 1~2개**에만 적용. 모든 재질에 반응을 넣으면 산만함. "갑옷 + 망토" 또는 "바닥 + 유리" 식으로 페어링.

**Reference presets** (when converting to video): `Turning Metal` / `Freezing` / `Ice Rose`.

---

## 베스트 프랙티스 룰

- **One primary FX category + 1~2 secondary** — 4개 이상은 충돌. Body/Motion artifact 룰의 정지 이미지 변형.
- **Action beat에 FX 묶기** — `at the moment of impact` / `mid-swing` / `at full extension`처럼 시점을 명시하면 모델이 "FX의 위상"을 잡음.
- **물리 동사 우선** — `rises / scatters / spray / erupt / emanate / drift / float / streak / coil / pulse`. 형용사 대신 동사로 풀어 쓸 것.
- **Color anchor + FX combo** — `cold blue moonlight + faint blue rune glow`처럼 색감과 FX를 한 색계로 묶으면 일관성↑.
- **비유 회피** — `ghost-like mist` ❌ → `thin gray mist drifting at knee height` ⭕. 모델은 비유보다 관측 가능한 묘사에 잘 반응.
- **방향성 명시** — `smoke` ❌ → `smoke curls upward slowly` ⭕. Optical/Fluid/Particle 모두 방향 누락 시 평면적.
- **고밀도 충돌 경고** — 한 컷에 4개 이상 카테고리 동시 등장 시 "어느 1~3개를 강조할지" 사용자에게 한 줄 질문.

---

## 경계 룰

- **영상 요청은 위임** — "이 장면을 영상으로", "도끼가 부딪히는 순간 뼈가 산산조각" 류의 요청은 본 스킬 범위 밖. Higgsfield 플랫폼의 영상 모션 프리셋(`Disintegration`, `Plasma Explosion`, `Glow Trace` 등)을 직접 쓰도록 안내.
- **단어수 보호** — augment 모드에서 본 프롬프트가 60단어를 넘거나 Cinema Studio 사용 명시 시 **512자 룰** 보호를 위해 FX 추가 전 사용자에게 단축/생략 여부 확인.
- **일반 artifact 회피 룰 준수** — Higgsfield 일반 가이드의 artifact 회피 패턴과 충돌하지 않게 어구 선택:
  - Body/Motion: 공중 분리된 사지·관통하는 무기 등 해부학 모순 묘사 금지
  - Texture/Lighting: 광원 충돌 (서로 다른 색의 키 라이트 동시 명시) 금지
  - Temporal/Consistency: contradictory motion (예: Dolly In + Dolly Out 동시) 패턴을 FX 묘사로도 만들지 않을 것
- **Soul 모델 케이스** — Soul 2.0/Soul Cinema Preview는 포토리얼이므로 Magical 카테고리 사용 시 "subtle / faint / barely visible" 같은 강도 조절 어휘를 묶어 과한 비현실감 방지.

---

## 주의사항

- **출력은 코드 블록 1개**만. 추가 설명·표·분해는 사용자가 명시 요청한 경우에만.
- **카테고리 강조 의도가 불분명**하면 한 줄 질문 ("Impact·Particle·Fluid·Optical·Atmospheric·Magical·Material 중 어떤 걸 강조할까요?").
- **영상 전용 프리셋 이름**(`Apply the Disintegration preset` 등)은 정지 이미지 프롬프트에 직접 넣지 않음 — 모델이 무시하거나 충돌. 프리셋의 시각 효과를 텍스트 어구로 풀어서 표현.
- **인종·연령 등 민감 속성**은 본 프롬프트에서 처리됨. FX 레이어에서는 다루지 않음.
- 사용자가 **한국어 자연어로 카테고리를 지정**하면 (예: "타격감, 입자감 추가") 영문 카테고리로 매핑 후 진행.
