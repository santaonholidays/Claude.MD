---
name: minimax-h3-video-prompt
description: Generate MiniMax H3 video prompts in T2VA, I2VA, FL2VA, L2VA or full-reference format. Use when the user asks for MiniMax H3 prompts, video generation prompts, T2VA, I2VA, first-last frame, last-frame, or reference-based video prompts. Keep descriptions literal and free of slang, focused on the most important actions and strict rule adherence. After generating any prompt, run a realism pass. For full-reference always evaluate compliance against the writing-guide rules.
---

# MiniMax H3 Video Prompt

Generate production-ready prompts for MiniMax H3 (Hailuo) following the official writing guides. Descriptions must stay literal, free of slang, and focused on the most important observable actions. After drafting, always run a realism pass. For full-reference mode, generate the six required sections then immediately evaluate every major rule from the guide.

## When to Use

- User wants a MiniMax H3 / Hailuo prompt
- Text-to-video (T2VA), image-to-video (I2VA), first-last (FL2VA), last-frame (L2VA)
- Full-reference mode (subjects, pictures, videos, audio references)

## Workflow

1. **Identify mode** from user request and assets:
   - No images → **T2VA**
   - One first-frame image → **I2VA**
   - First + last frame images → **FL2VA**
   - One last-frame image → **L2VA**
   - Multiple subjects / reference images / videos / audio → **full-reference**

2. **Load the correct guide**:
   - T2VA / I2VA / FL2VA / L2VA → read `references/VIDEO_PROMPT_WRITING_GUIDE_base_en.md`
   - Full-reference → read `references/VIDEO_PROMPT_WRITING_GUIDE_ref_en.md` (and the shared core rules in the base guide sections 4.1–4.7)

3. **Write the prompt** exactly in the required structure. Do not invent extra sections or change field names. Apply the Literal Language rules below while writing.

4. **Realism pass** (mandatory for every mode):
   After the initial draft, re-read the full prompt and revise it for physical and cinematic realism. Ensure action timing feels natural, motions obey basic physics and inertia within the stated style, lighting and object interactions stay consistent, and sequences could plausibly be filmed or rendered. Soften or remove exaggerated, impossible, or artificial-feeling elements that the user did not explicitly request. Prefer concrete, observable details. Keep the same structure and labels; only improve realism and naturalness.

5. **Output the prompt** (the version after the realism pass) inside a single ```text code block.

6. **For full-reference mode only — mandatory evaluation**:
   After the code block, produce a **Rule Compliance Evaluation** section. Check every rule listed under “Full-Reference Rule Checklist” below. For each rule state PASS / FAIL / N/A and a one-line note. If any FAIL exists, immediately revise the prompt and re-evaluate until all critical rules pass. Do not leave residual violations. Re-run a quick realism check on any revised text.

7. Keep any help text after the evaluation short and scannable. List only the files the user must attach.

## Output Format (all modes)

```text
[the complete prompt here]
```

**Attach these files** (list only what is needed):
- Picture 1 / first frame: ...
- Picture 2 / last frame: ...
- Subject / reference images: ...
- Reference videos / audio: ...

## Full-Reference Rule Checklist

Use this checklist for the post-generation evaluation. Source of truth is `references/VIDEO_PROMPT_WRITING_GUIDE_ref_en.md` plus the shared rules in the base guide.

### Structure & Language
- [ ] Exactly six sections in order: `subject_definitions`, `summary`, `retention_analysis`, `detailed_description`, `overall_soundscape`, `non_diegetic_music`
- [ ] All sections written in English; original language preserved only inside `<d>` and for on-screen text
- [ ] No extra sections or renamed fields

### subject_definitions
- [ ] Every tracked piece of content has its own line and stable label (`<Subject N>`, `<Picture N>`, `<Video N>`, `<Audio N>`)
- [ ] Standalone `<Picture N>` only when the image is a concrete first/key/last frame or composition anchor; otherwise cite the image inside the corresponding `<Subject N>`
- [ ] `<Video N>` only for whole-video editing, continuation, or structural reference; visible content from a video still uses `<Subject N>`
- [ ] `<Audio N>` defined with role; when bound to a speaker, reuse the target speaker ID `(Sx)` (never invent a new one)
- [ ] Labels keep the same meaning across all later sections
- [ ] Prefer lean, role-focused definitions that state what each reference asset supplies (e.g. “facial identity from <Picture 2>, body and clothing from <Picture 1>”) rather than exhaustive re-description of details the images already own. Over-describing static appearance can compete with the reference pixels and weaken identity lock.

### summary
- [ ] Begins with a square-bracketed task-type prefix (e.g. `[reference generation]`, `[video editing + audio reuse]`)
- [ ] Uses only previously defined labels; no new labels introduced
- [ ] For video-editing tasks starts with “The target video is an edited version of <Video N>.”
- [ ] Short single paragraph

### retention_analysis
- [ ] One line per defined label
- [ ] Visual labels use only: `fully_preserved` / `partially_preserved` / `attribute_transfer` / `weak_reference`
- [ ] Audio labels use only: `fully_copy` / `partially_copy` / `reference` / `weak_reference`
- [ ] Each line states the shots (or role) where the content appears and a brief justification

### detailed_description
- [ ] 1–2 English style sentences before `[Shot 1]` (full-ref style opening)
- [ ] `[Shot 1]` has no timestamp; later shots use `[Shot N] At MM:SS.mmm, ...` with strictly increasing times
- [ ] Every shot describes: current composition, subject appearance & position, environment & lighting, actions & state changes, camera movement, current sound, and where referenced labels take effect
- [ ] Not a plot summary or a mere list of reference relationships; maximally explicit and visual
- [ ] Generation tasks: normally 350–500 English words (dialogue-dense content prioritizes complete spoken timeline)
- [ ] Reference labels inserted at first clear appearance and wherever their role applies
- [ ] Camera motion written as natural English (type + amplitude + speed when meaningful); never stacked labels
- [ ] Speakers use stable `(S1)`, `(S2)`…; when a defined subject speaks write `<Subject N> (Sx)`
- [ ] Dialogue / lyrics inside `<d>[Language] exact original text</d>`; preserve every original word and punctuation; standardize only decorative marks
- [ ] Voice-over uses the exact phrase “says in an off-screen voiceover” and states lips remain closed
- [ ] Dialogue crossing a cut uses `<scenetrans>` + continuity wording; truncated speech uses `<cutoff>`
- [ ] Visible on-screen text placed in English double quotes, original wording preserved
- [ ] Shared camera, speaker, and continuity rules from base guide §4.2–4.5 obeyed

### overall_soundscape & non_diegetic_music
- [ ] `overall_soundscape`: 1–4 sentences of ambience + physical + non-verbal human sounds only; dialogue/singing/diegetic music stay in detailed_description
- [ ] `non_diegetic_music`: 1–3 sentences focused on instrumentation, tempo, rhythm, dynamics; no abstract mood words; `N/A` when absent
- [ ] When reference audio is used, the copy/reference relationship is stated in the matching section (ambience → soundscape, score → music)
- [ ] No dialogue or lyrics repeated in these two sections

### General
- [ ] Labels never redefined after subject_definitions
- [ ] No invented speaker IDs for pure BGM/lyric cues that are only part of a copied `<Audio N>`
- [ ] Task-type combination uses ` + ` and never repeats a type

## Literal Language Rules (all modes)

- Write every description in plain, literal English. No slang, no idioms, no metaphors, no figurative or poetic language.
- Focus on the most important actions, positions, movements, and visible state changes. Avoid padding with minor decorative details that do not drive the shot.
- Prefer concrete, observable wording (what can be seen or heard) over abstract, emotional, or interpretive language.
- Strictly adhere to the field order, naming, and constraints of the loaded writing guide. Rule adherence takes priority over stylistic flourish.

## Rules (all modes)

- Follow the guide’s field names, order, and phrasing exactly.
- Preserve original dialogue/lyrics inside `<d>[Language] ...</d>` verbatim.
- Use stable speaker IDs `(S1)`, `(S2)`.
- Camera motion = type + amplitude + speed in natural English.
- `overall_soundscape` and `non_diegetic_music` are mandatory; use `N/A` only when appropriate.
- For keyframe modes, the instruction line must be the first line of the prompt.
- After every draft, apply the mandatory realism pass described in the Workflow before outputting the final prompt.

## Best practices for reference strength (full-reference & I2VA)

- In `subject_definitions`, keep definitions lean and role-focused. State *what each asset supplies* (identity, clothing, motion, voice) rather than re-listing every visual attribute the image already contains.
- Official examples are concise (e.g. “the young woman in <Picture 1>, with long dark hair, a blue cardigan…”). Exhaustive text descriptions of face, hair, embroidery, etc. can compete with the actual pixels and weaken identity lock, especially when a dedicated high-resolution face reference is provided.
- In `detailed_description`, still establish subject appearance & position at first clear appearance (required), but keep the reminder short and always tie it back to the `<Subject N>` + source labels. Thereafter lean on the labels.
- Assign every reference a clear, non-overlapping job. Ambiguous or competing descriptions reduce the authority of the strongest reference (usually a high-res face or product shot).

## Quick Mode Checklist

| Mode | First line of prompt | Core fields |
|------|----------------------|-------------|
| T2VA | none | integrated_multimodal_description / overall_soundscape / non_diegetic_music |
| I2VA | `For the target video, at 0.00 seconds... <Picture 1> (from [Shot 1]) is fully referenced.` | same three fields |
| FL2VA | `How the reference pictures align... Picture 1 ... 0.00-second; Picture 2 ... S.SS-second.` | same three fields |
| L2VA | `How the reference pictures align... <Picture 1> (from [Shot N]) aligns with the S.SS-second mark...` | same three fields |
| Full-ref | (no special first line) | subject_definitions / summary / retention_analysis / detailed_description / overall_soundscape / non_diegetic_music |

Duration in alignment lines is always formatted to two decimal places (e.g. 6.00, 8.00).

## References

- Base modes (and shared core rules §4): `references/VIDEO_PROMPT_WRITING_GUIDE_base_en.md`
- Full-reference: `references/VIDEO_PROMPT_WRITING_GUIDE_ref_en.md`
