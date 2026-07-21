# 00_agent_workflow.md

This is the operating manual. Read this file + `channel_identity.yaml` +
`seo_rules.yaml` + `content_pillars.yaml` + `ai_generation_rules.yaml` before
starting or continuing any episode. Everything else (per-episode files) is
data these steps produce or consume.

Do not re-explain these rules back to the user. Just follow them.

---

## STEP 0 — Load context

Read all five core files above once. Check `asset_library/registry.yaml` for
any characters/locations/objects that already exist and could be reused
(e.g. recurring investigator, recurring location type). Reuse before creating new.

---

## STEP 1 — Topic

Ask the user which story/topic this episode covers, unless they already
stated it.

- If the user already has research, notes, or a specific angle in mind →
  take it as-is, do not re-research from scratch, just fill gaps.
- If the user has nothing prepared → research it autonomously (web search),
  then present a short topic proposal (title idea, category from
  `content_pillars.yaml`, main question, why it matters) for confirmation
  before going further.
- Score the topic against `content_pillars.yaml -> topic_scoring`. If it's
  clearly below threshold, say so before investing more effort.

---

## STEP 2 — Research

Produce `research.md` (prose — this needs human judgment, keep it as
readable markdown, not YAML). Structure: timeline, people, locations,
evidence, theories. Separate confirmed fact from speculation explicitly.

---

## STEP 3 — Script

Produce `script.md` (prose). Follow
`channel_identity.yaml -> episode_structure_template` and
`narrative_voice`. Target length from `content_pillars.yaml -> episode_length`
unless the user specified otherwise.

---

## STEP 4 — Characters & Assets

Produce `characters.yaml` and `assets.yaml` for this episode.

- For each character/location/object, first check
  `asset_library/registry.yaml`. If an equivalent already exists
  (e.g. a generic "FBI investigator" type, a generic airport interior),
  reference it instead of creating a new one.
- Only create new entries for what's genuinely new to this episode.
- New reusable entries get built in Blender and added to
  `asset_library/registry.yaml` so future episodes can reuse them.
- Follow `ai_generation_rules.yaml` for any entry that will be produced via
  AI generation instead of Blender (mark it `production_method: ai_generated`
  vs `production_method: blender`).

---

## STEP 5 — Blueprint

Produce `blueprint.yaml` (structured, not prose — this is what the
production scripts will parse). One entry per shot:

```yaml
- scene: 1
  shot: 1
  time: ["00:00", "00:06"]
  narration: "..."
  characters: [CH-001]
  assets: [LOC-001, OBJ-001]
  camera: {type: wide establishing, lens: 35mm, movement: slow tracking from behind, angle: eye level}
  lighting: "..."
  direction: "..."
  voice_direction: {emotion: calm, speed: slow, pause_after: "briefcase"}
  music: AUD-010
  production_method: blender   # or ai_generated
  editing: slow cut
```

---

## STEP 6 — SEO Metadata (always automatic, do not wait to be asked)

The moment the blueprint is locked, generate `metadata.yaml` using
`seo_rules.yaml`. This step is not optional and not a separate request —
it's a standard deliverable of every episode, same as the script.

Produce:
- 3 ranked title options
- 1 final description (150-300 words)
- 15-20 tags
- 2-3 thumbnail concept briefs
- full chapter timestamp list (once shot timings are final)

---

## STEP 7 — Production

Follow the hybrid pipeline: Blender for anything tagged
`production_method: blender` (consistent, reusable, faster to iterate),
AI generation for anything tagged `production_method: ai_generated`
(one-off complex shots — storms, crowds, archival inserts).

Recommended production order (from original phase guidelines — still applies):
1. Generate/build reference images for any new character or asset
2. Lock character consistency (confirm against `characters.yaml` / registry before mass-producing shots)
3. Generate/build environments
4. Generate video shots
5. Generate narration (voice)
6. Create sound design

Assemble with FFmpeg/Remotion per the blueprint shot order.

Editing priorities, in order:
1. **Story first** — never sacrifice storytelling for visuals
2. **Pacing** — protect curiosity, emotional progression, viewer retention
3. **Sound** — audio quality has equal importance to visuals, not an afterthought

---

## STEP 8 — QC

Before marking an episode done, check all four categories:

- **Story**: Is information accurate? Is the timeline correct?
- **Visuals**: Are characters consistent? Are historical details correct?
- **Audio**: Is narration clear? Are sound effects realistic?
- **Viewer experience**: Does the opening hook work? Is the pacing engaging? Is the ending memorable?

Also run `channel_identity.yaml -> pre_publish_quality_checklist` as a final pass.

---

## STEP 9 — Publishing

Upload checklist — confirm all present before publishing:
- **Title**: clear + curiosity-driven (from `metadata.yaml`)
- **Thumbnail**: readable and emotional (from `metadata.yaml` thumbnail_concepts)
- **Description**: summary + keywords + sources (from `metadata.yaml`)
- **Metadata**: tags, category, chapters complete (from `metadata.yaml`)

---

## STEP 10 — Post-publishing (ongoing, after upload)

Review once data is available: click-through rate, average view duration,
audience retention, comments. Use this to improve future episodes — every
subsequent episode should improve at least one of: storytelling, visual
quality, production speed, or audience engagement. This is a standing habit,
not a one-time step — revisit it before starting each new episode.

---

## Ground rules across all steps

- Don't produce heavy documentation for its own sake. Every file exists to
  either get produced into video or to keep production consistent. If a
  file isn't feeding one of those two purposes, don't create it.
- Prose only where creative/narrative judgment is needed (research, script).
  Everything structured/repeatable (characters, assets, blueprint, metadata)
  is YAML.
- Never restate character/asset descriptions inside the blueprint — ID
  reference only.
- Ask the user only when a real decision is needed (topic confirmation,
  missing research, ambiguous creative direction). Don't ask about things
  already answered by the core files.
