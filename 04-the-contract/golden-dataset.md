# Golden Dataset & Reliability Contract

**Product: Wavelength — environment classifier (`eval/environment-classifier.test.ts`)**

## Golden Dataset Spec

| # | Input Type | Input | Expected Output | Edge Case? | Judge Type |
|---|-----------|-------|----------------|-----------|-----------|
| 1 | System | BG noise: 71dB · chatter density: high · reverb: med · speech-to-noise: 0.35 · time: 19:40 · GPS: restaurant district | Restaurant | N | rule |
| 2 | System | BG noise: 42dB · chatter density: low · reverb: low · speech-to-noise: 0.8 · time: 10:15 · GPS: office building | Office | N | rule |
| 3 | System | BG noise: 55dB · wind: high · birdsong: present · reverb: none · GPS: park | Outdoors | N | rule |
| 4 | System | BG noise: 63dB · engine hum: present · road noise: high · GPS: moving, 45mph | Car | N | rule |
| 5 | System | BG noise: 38dB · TV audio: present · reverb: low · GPS: home address | Home | N | rule |
| 6 | User | User manually labels this location "Mom's house" after 3 visits | Home (user-labeled variant, merged with acoustic profile) | N | rule |
| 7 | System | BG noise: 51dB · chatter density: low-medium · reverb: low · speech-to-noise: 0.65 · GPS: coworking space | Office | N | rule |
| 8 | System | BG noise: 58dB · chatter density: medium · reverb: medium · speech-to-noise: 0.5 · GPS: unavailable (indoors, no signal) · time: 13:00 | Restaurant (low confidence) | Y | both |
| 9 | System | BG noise: 0dB (silence) · all sensors nominal · GPS: unavailable | Unknown — abstain, do not force-classify | Y | LLM |
| 10 | System | BG noise: 66dB · chatter density: high · reverb: high · engine hum: faint · GPS: moving, 5mph (drive-thru) | Car (conflicting Restaurant + Car signals) | Y | LLM |
| 11 | System | BG noise: 82dB · chatter density: very high · music: loud, bass-heavy · reverb: high · GPS: unavailable | Unknown — environment outside current taxonomy (concert/bar) | Y | LLM |
| 12 | System | BG noise: 45dB · chatter density: low · reverb: low · speech-to-noise: 0.75 · GPS: office building · time: 22:40 | Office (acoustic signal should override time-of-day heuristic) | Y | both |
| 13 | System | BG noise: 59dB · chatter density: medium · reverb: medium · speech-to-noise: 0.55 · GPS: shopping mall, food court zone | Restaurant (borderline vs. general Public/Outdoors) | Y | LLM |
| 14 | System | BG noise: 70dB · wind: extreme (gust interference) · sensor clipping detected · GPS: outdoors, stadium | Outdoors (must stay robust to corrupted/clipped signal) | Y | LLM |
| 15 | System | BG noise: 60dB · engine hum: present · road noise: medium · chatter density: high (passengers talking) · GPS: moving, 30mph | Car (borderline vs. Restaurant on chatter density alone) | Y | both |

**Dataset health:** 15 total test cases · 8 edge cases (53%, above the 20% target) · judge mix Rule 7 / Both 3 / LLM 5 · all 5 known environment labels covered plus one user-input override case.

**Adversarial rows included:** 3 — dead silence (#9, should abstain rather than hallucinate a label), an environment outside the current taxonomy (#11, concert/bar), and corrupted/clipped sensor data (#14, tests robustness rather than crashing or silently mislabeling).
**Coverage gaps identified by partner:**

## Confidence UX Design

**Approach:** show uncertainty / tiered confidence / human-in-loop trigger
Tiered confidence, combined with visible reasoning at each level and a human-in-loop trigger at the low end — the classifier never silently forces a wrong sound profile onto someone's ears, since a bad hearing adjustment is felt immediately, unlike a bad text suggestion.

**High confidence (>90%):**
Auto-apply the learned profile silently. The environment pill shows the detected name (e.g., "Restaurant") with no confirmation step. The user can still open it and manually adjust at any time — that adjustment becomes a correction event, not an error report.

**Medium confidence (70-90%):**
Apply the best-guess profile, but visibly soften it — the pill renders as "Restaurant?" with a dashed border, plus a one-tap "not quite?" affordance. Tapping surfaces the top 2 candidate environments (e.g., Restaurant vs. Food Court) so the user can confirm in a single gesture rather than fully re-adjusting sliders from scratch.

**Low confidence (<70%):**
Don't auto-apply any learned profile — fall back to the user's manual/default settings, and show a lightweight "New place? Tell us where you are" prompt instead of guessing. This is the human-in-loop trigger: the one-tap answer becomes a new labeled example fed back into the golden dataset, rather than risking a wrong, uncomfortable setting.

**User control surface:**
- **Users adjust the confidence threshold — Y (indirectly):** exposed as a simple "Trust new guesses: Cautious / Balanced / Confident" preset rather than a raw percentage — a numeric slider isn't meaningful to most hearing aid wearers.
- **Users see AI reasoning/drivers — Y:** tapping the environment pill shows the acoustic signals behind the call in plain language (e.g., "high chatter, low reverb, GPS: restaurant district"), since trust in a sound change requires understanding why it happened.
- **Users correct & override outputs — Y:** every applied profile stays adjustable via the sliders on the main dashboard; that adjustment is itself the correction signal (the Correction/Preference loops already scored 4/5 in the flywheel).
- **Corrections feed back into the model/dataset — Y:** this is the exact fix flagged as the 48-hour action in the Kill Switch audit (currently Locked/no capture) — every override gets logged as a new labeled row in the golden dataset.
- **Specifics:** every environment pill carries a one-tap correction with reason codes — "wrong environment," "right environment, wrong profile," "no environment fits" — which append directly to the `environment-classifier` golden dataset and retrain the per-user profile within 24 hours.

## Reliability Contract

| Metric | Target | Measurement | Alert Threshold |
|--------|--------|-------------|-----------------|
| Accuracy | 90% | Weekly · golden dataset (currently 15 rows, scaling to 150+) · rule-judge for clean matches + LLM-judge (GPT-4o) for ambiguous/adversarial rows, per the Judge Type column above | <85% → pages on-call PM |
| Hallucination rate | <2% | Same weekly run · flags any row where the model outputs a confident label instead of abstaining on the adversarial/silence/out-of-taxonomy test rows (#9, #11, #14) | >3% → pauses auto-apply in prod, rolls all users back to confirm-before-apply mode |
| Latency (p95) | <300ms | Continuous prod monitoring (Datadog) on the classification call path — must be near-instant since this fires the moment a user walks into a room, not on a request they're waiting on | >600ms for 5 min → pages on-call engineer |
| Drift velocity | <0.5%/wk | 4-week rolling accuracy trend against the golden set | >1%/wk decay → triggers gold-set audit |

**Defensible bands:** Accuracy 88-93 (not 99) · Hallucination <2% · Latency <300ms · Drift <0.5%/wk
**Consequence patterns:** 🔄 Auto-rollback to confirm-before-apply · 📟 Pages on-call PM/engineer · 🔍 Gold-set audit · 🧑 Human queue (one-tap environment prompt)

## HITL Architecture
<!-- When does a human step in? What's the escalation path? -->
Confidence <70% OR a flagged adversarial pattern (silence, out-of-taxonomy environment, corrupted sensor signal) → don't auto-apply, prompt the user with the lightweight "Where are you?" one-tap flow instead of guessing. The user's answer feeds back into the golden dataset within 24 hours and updates that user's personal profile immediately — the same correction loop that feeds the weekly gold-set audit also absorbs this human-in-loop input, so there's a single pipeline rather than two parallel ones.

## Red-Team Findings
*What failure mode did your partner find that you missed?*
