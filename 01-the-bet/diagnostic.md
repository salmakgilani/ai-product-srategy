# Three-Axis Vulnerability Diagnostic

## Product
<!-- Name the product you're diagnosing. Real product at your company — not a hypothetical. -->

**Product:** Wavelength — Context-Adaptive Listening Copilot for hearing aid users
**Your Role:** Product Manager, exploratory bet

---

## Scores

### Contextual Moat — 4/5
*Workflow depth × switching cost. Would users leave in a weekend if a competitor showed up?*

**Score rationale:**
Wavelength sits on a device the user wears all day, continuously adjusting sound profiles per environment (restaurant, office, outdoors). After months of use, the app holds a set of trained, personalized profiles — switching to a competitor means starting from zero, and in hearing, a degraded or generic-sounding experience is immediately and viscerally noticeable. That's a high switching cost built from daily, non-optional use.

**Named attacker (from partner challenge):**
The hearing aid OEM itself (Phonak, Oticon, ReSound) — they already own the Bluetooth/firmware relationship and could bundle equivalent personalization directly into their first-party app.

---

### Data Advantage — 4/5
*Proprietary signal that compounds with usage. What do you see that OpenAI doesn't?*

**Score rationale:**
The core signal — manual volume/clarity/noise-reduction corrections tagged by environment — is genuinely personal and not obtainable through any public API. Today's OEM apps are thin volume-adjustment UIs, not learning systems, so there's a real head start in building a model that improves per-user the longer someone uses it.

**Named attacker (from partner challenge):**
Phonak myPhonak / Oticon ON — the existing OEM companion apps, if they invest in an adaptive-learning layer on top of the device data they already have.

---

### Platform Exposure — 2/5
*Encroachment risk × pivot speed. If Apple/Google/OpenAI ships your hero feature native — then what?*

**Score rationale:**
Real mid-term risk. Apple is aggressively expanding native hearing-health features (AirPods Pro clinical-grade hearing aid mode, Live Listen, Conversation Boost) with full audio-pipeline access and default distribution to hundreds of millions of devices. The mitigating factor: this doesn't yet cover prescription hearing aids from third-party OEMs (Phonak, Oticon, ReSound, Widex, Signia), which fragments the market and buys real time.

**Named attacker (from partner challenge):**
Apple (AirPods Pro Hearing Health / Live Listen).

---

## Top Vulnerability
<!-- One line: what's the single biggest strategic risk? -->
Apple's expansion of native hearing-health features into AirPods Pro — full audio-pipeline access plus massive default distribution — is the most credible long-term threat, even though it doesn't yet reach prescription third-party hearing aid wearers.

## Confidence Level
<!-- H / M / L — how confident are you in this bet after the diagnostic? -->
M — a real vulnerability, but slower-moving and narrower in scope than a platform that already owns the exact data source (as with Signal/Salesforce), leaving a real window to build defensible personalization data first.
