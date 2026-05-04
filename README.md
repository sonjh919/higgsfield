# higgsfield

Claude Code skills for working with [Higgsfield AI](https://higgsfield.ai) prompts.

Currently bundles two skills that work together:

- **`higgsfield-fx`** — A focused FX (visual effects) layer builder for **still-image** Higgsfield prompts. Provides English vocabulary and phrasing across 7 categories (Impact / Particle / Fluid / Optical / Atmospheric / Magical / Material), plus collision-avoidance and density rules. Designed as a self-contained **add-on** that augments prompts produced by your existing Higgsfield prompt workflow rather than replacing it.
- **`higgsfield-fx-tuner`** — A meta-skill that lets `higgsfield-fx` evolve to fit your usage. As you give natural-language feedback while using `higgsfield-fx` ("이 glow 너무 강해", "이런 어구 추가해줘", "이건 빼줘"), it quietly logs each piece of feedback. After 7 items have accumulated — or when you run `/higgsfield-fx-tuner review` — it analyses the log, picks out repeating patterns (3+ occurrences become rule/vocab candidates), discards one-off opinions, asks you about any contradictions, and applies the curated changes to `higgsfield-fx/SKILL.md` in one batch. Every change is backed up automatically and recorded in a human-readable `CHANGELOG.md`.

## What `higgsfield-fx` does

Two input modes, both of which can take a reference image as auxiliary input:

| Mode | Input | Output |
| --- | --- | --- |
| `augment` | An existing English prompt + (optional) emphasized categories | A single prompt with FX phrases inserted between `lighting` and `color palette` |
| `standalone` | Scene context (natural language) + emphasized categories | Labeled English FX phrases (1–3 per category) ready to merge into your own prompt |

When a reference image is attached, the skill reads it multimodally to extract already-visible FX, materials, light direction, and tone — and avoids duplicating what the image already shows.

## What `higgsfield-fx-tuner` does

Four modes, all driven from natural-language Korean (no slash command needed for the most common one):

| Mode | Trigger | Action |
| --- | --- | --- |
| `capture` | Auto — natural-language feedback like "이거 별로야" | Appends one line to `feedback-log.md`, replies with `📝 피드백 기록됐어요 (N/7)` |
| `review` | `/higgsfield-fx-tuner review` (or after threshold of 7) | Analyses feedback, shows a Korean change preview, applies on approval, updates `SKILL.md` + `CHANGELOG.md`, makes a backup |
| `report` | `/higgsfield-fx-tuner report` | Outputs a single Korean text block summarising what evolved, ready to copy into a chat or email |
| `status` | `/higgsfield-fx-tuner status` | Shows current feedback count, last curation date, last change, file sizes |

The split between *capture* (cheap, every interaction) and *review* (deliberate, on demand) keeps `SKILL.md` lean — only patterns that recur 3+ times get promoted into rules or vocabulary, so the file does not bloat with every casual remark.

## Install — option A: GitHub clone (developers)

```bash
# one-shot copy
git clone https://github.com/sonjh919/higgsfield /tmp/higgsfield-share
cp -r /tmp/higgsfield-share/higgsfield-fx /tmp/higgsfield-share/higgsfield-fx-tuner ~/.claude/skills/

# or symlink so `git pull` propagates updates
git clone https://github.com/sonjh919/higgsfield ~/repos/higgsfield
ln -s ~/repos/higgsfield/higgsfield-fx       ~/.claude/skills/higgsfield-fx
ln -s ~/repos/higgsfield/higgsfield-fx-tuner ~/.claude/skills/higgsfield-fx-tuner
```

Restart your Claude Code session. Both skills should appear in the user-invocable skills list on the next prompt.

## Install — option B: ZIP / shared folder (for non-developers)

Don't use GitHub? Ask whoever shared this with you to send you a folder containing the two directories: `higgsfield-fx/` and `higgsfield-fx-tuner/`. Then:

1. Open a terminal (macOS: 응용 프로그램 > 유틸리티 > 터미널 / Windows: PowerShell).
2. Make sure `~/.claude/skills/` exists. (It is created automatically the first time you run Claude Code.)
3. Copy both folders in:

   **macOS / Linux**
   ```bash
   cp -r ~/Downloads/higgsfield-fx ~/Downloads/higgsfield-fx-tuner ~/.claude/skills/
   ```

   **Windows (PowerShell)**
   ```powershell
   Copy-Item -Recurse $HOME\Downloads\higgsfield-fx,$HOME\Downloads\higgsfield-fx-tuner $HOME\.claude\skills\
   ```

4. Restart Claude Code.

## 친구를 위한 한국어 가이드

이 스킬을 처음 받으셨나요? 아래 한 단락이면 충분합니다.

> Claude Code 안에서 평소처럼 말씀하시면 됩니다. 영문 프롬프트가 필요하면 `higgsfield-fx`가 알아서 만들어주고, 결과가 마음에 안 들 때 한국어로 자연스럽게 피드백("glow가 너무 강해", "이런 어구도 추가해줘", "이건 빼줘")을 주시면 `higgsfield-fx-tuner`가 조용히 메모해뒀다가 7개쯤 모이면 "정리해드릴까요?" 하고 물어봅니다. "예"라고 답하시면 본인의 사용 패턴에 맞게 스킬이 알아서 업그레이드됩니다.
>
> 변경 이력이 궁금할 때는 `/higgsfield-fx-tuner status`, 다른 사람한테 보고서로 보내고 싶을 때는 `/higgsfield-fx-tuner report`를 입력하세요.

**백업 권장**: `~/.claude/skills/higgsfield-fx` 폴더 통째로 가끔 클라우드(iCloud / 구글 드라이브 등)에 복사해두세요. 노트북이 날아가면 그동안 쌓인 진화도 같이 사라집니다.

## Usage examples

**Standalone with image** — attach a reference image and describe the FX you want:

> [image] 무기에 황금빛 에너지 발광 추가, 머리 제외 신체 아우라

**Augment an existing prompt**:

> 이 프롬프트에 Impact + Material 강조해서 FX 보강해줘
>
> "A medieval knight in steel chainmail hauberk swings a heavy double-edged battle axe..."

**Feedback flow**:

> (생성된 결과 보고 나서) 이 glow가 너무 강해
> → 📝 피드백 기록됐어요 (3/7)
>
> ... (나중에)
> /higgsfield-fx-tuner review
> → 📋 7개 피드백 분석 결과 ... (변경안 미리보기 후 "예" 입력 → 일괄 반영)

## Categories at a glance

| Category | Examples |
| --- | --- |
| Impact | sparks scatter, dust burst, fragment trajectory, shock ring |
| Particle | embers floating, ash drifting, dust motes suspended, snow crystals tumbling |
| Fluid | steam rises, smoke curls, breath fog, blood mist, flames lick |
| Optical | lens flare, anamorphic streak, bloom, motion blur trail, god rays |
| Atmospheric | fog rolling in, haze softening the distance, rain streaks, heat haze |
| Magical / Energetic | glow emanates, runes pulsing, energy trail, eye luminance |
| Material reaction | chainmail rippling, fabric flutter, glass crack pattern, frost spreading |

Density rule: one primary category + 1–2 secondary. Asking for 4+ at once triggers a clarifying question to narrow scope.

## Scope

- **In scope**: still-image FX layering for any Higgsfield image model (Soul 2.0, Soul Cinema Preview, Nano Banana 2, Kling Image 3.0, Seedream, etc.); per-user evolution of vocabulary and rules
- **Out of scope**: video FX (Higgsfield's video-side motion presets like `Disintegration`, `Plasma Explosion` cover that), non-photoreal style overlays, the main prompt skeleton (Subject / Camera / Action), and propagating one user's evolution to the upstream repo (that's a manual decision based on the report output)

## License

MIT — see [LICENSE](./LICENSE).
