# higgsfield

Claude Code skills for working with [Higgsfield AI](https://higgsfield.ai) prompts.

Currently includes:

- **`higgsfield-fx`** — A focused FX (visual effects) layer builder for **still-image** Higgsfield prompts. Provides English vocabulary and phrasing across 7 categories (Impact / Particle / Fluid / Optical / Atmospheric / Magical / Material), plus collision-avoidance and density rules. Designed as an **add-on** that augments prompts produced by the main Higgsfield prompt skill (e.g. [OSideMedia/higgsfield-ai-prompt-skill](https://github.com/OSideMedia/higgsfield-ai-prompt-skill)) rather than replacing them.

## What `higgsfield-fx` does

Two input modes, both of which can take a reference image as auxiliary input:

| Mode | Input | Output |
| --- | --- | --- |
| `augment` | An existing English prompt + (optional) emphasized categories | A single prompt with FX phrases inserted between `lighting` and `color palette` |
| `standalone` | Scene context (natural language) + emphasized categories | Labeled English FX phrases (1–3 per category) ready to merge into your own prompt |

When a reference image is attached, the skill reads it multimodally to extract already-visible FX, materials, light direction, and tone — and avoids duplicating what the image already shows.

## Install (Claude Code)

This repo is a single skill in a subdirectory. The cleanest install is to clone the repo somewhere temporary and copy the skill folder into your Claude Code skills directory:

```bash
git clone https://github.com/sonjh919/higgsfield /tmp/higgsfield-share
cp -r /tmp/higgsfield-share/higgsfield-fx ~/.claude/skills/
```

Or, if you want to symlink so updates pull in via `git pull`:

```bash
git clone https://github.com/sonjh919/higgsfield ~/repos/higgsfield
ln -s ~/repos/higgsfield/higgsfield-fx ~/.claude/skills/higgsfield-fx
```

After install, restart your Claude Code session. The skill registers as `higgsfield-fx` and is invocable via `/higgsfield-fx` or by natural-language requests like "add FX to this prompt", "이펙트 보강해줘", "타격감/파편/입자/안개/glow/lens flare 추가".

## Usage examples

**Standalone with image** — attach a reference image and describe the FX you want:

> [image] 무기에 황금빛 에너지 발광 추가, 머리 제외 신체 아우라

**Augment an existing prompt**:

> 이 프롬프트에 Impact + Material 강조해서 FX 보강해줘
>
> "A medieval knight in steel chainmail hauberk swings a heavy double-edged battle axe..."

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

- **In scope**: still-image FX layering for any Higgsfield image model (Soul 2.0, Soul Cinema Preview, Nano Banana 2, Kling Image 3.0, Seedream, etc.)
- **Out of scope**: video FX (use `higgsfield-motion` from OSideMedia for named presets like `Disintegration`, `Plasma Explosion`), non-photoreal style overlays (use `higgsfield-mixed-media`), the main prompt skeleton (Subject / Camera / Action — covered by `higgsfield-prompt`)

## License

MIT — see [LICENSE](./LICENSE).
