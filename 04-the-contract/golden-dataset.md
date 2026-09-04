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

**High confidence (>90%):**
**Medium confidence (70-90%):**
**Low confidence (<70%):**

**User control surface:**

## Reliability Contract

| Metric | Target | Measurement | Alert Threshold |
|--------|--------|-------------|-----------------|
| Accuracy | | | |
| Hallucination rate | | | |
| Latency (p95) | | | |
| Drift velocity | | | |

## HITL Architecture
<!-- When does a human step in? What's the escalation path? -->

## Red-Team Findings
*What failure mode did your partner find that you missed?*
