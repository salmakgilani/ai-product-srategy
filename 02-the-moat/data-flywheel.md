# Data Flywheel Map

> Score each loop 1-5. Your weakest loop is where competitors attack first.
> The four loops below are the M2 starting point - adapt if your product has 2 or 6 loops instead of 4.

## Flywheel Loops

**Product: Wavelength — Context-Adaptive Listening Copilot**

| Loop | What It Measures | Score 1 | Score 5 | Score |
|------|------------------|---------|---------|-------|
| **Correction** | Do users fix AI outputs? Is that signal captured and reused? | No capture | Automated retraining | 4/5 |
| **Preference** | Does the product learn individual / team preferences over time? | Stateless | Deep personalization | 4/5 |
| **Domain Context** | Does usage in one area improve quality in adjacent areas? | Siloed | Cross-domain transfer | 3/5 |
| **Network** | Does each new user / team make the product better for everyone? | Isolated | Strong network effects | 3/5 |

### Correction Loop - 4/5
**What you capture today:** Every manual volume/clarity/noise-reduction tweak a user makes, tagged with context — location type, time of day, ambient noise level.
**How it compounds:** Each correction directly retrains that user's environment-specific profile, so accuracy in that context improves within days of repeated use.
**How this changes your future experience:** The app gets quieter and more "invisible" over time — fewer manual adjustments are needed because it anticipates the right settings automatically.

### Preference Loop - 4/5
**What you capture today:** Individual sound profiles per environment per user, plus which environments they frequent most.
**How it compounds:** Deep personalization — two users with similar hearing-loss profiles in similar environments may start from smarter defaults, but each individual's model keeps sharpening the longer they use it.
**How this changes your future experience:** The app increasingly reflects "how I like to hear," not a generic hearing-aid default — highly sticky and hard for a new entrant to shortcut.

### Domain Context Loop - 3/5
**What you capture today:** Environment tags (restaurant, office, outdoors, car) correlated with acoustic signatures — frequency profile, background noise pattern.
**How it compounds:** Some cross-domain transfer is plausible — a learned response to "loud, echoey rooms" from restaurants could reasonably seed a starting point for a similarly acoustic gym or lobby, without a fully fresh training cycle.
**How this changes your future experience:** New environments feel less like starting over — the app has a reasonable guess before you've corrected it even once.

### Network Loop - 3/5
**What you capture today:** Potential for anonymized, opted-in aggregate corrections across users with similar audiograms and environments, used to set smarter defaults for brand-new users.
**How it compounds:** Each new consenting user marginally improves the default starting profile offered to future users with similar hearing-loss patterns — a real, if modest, network effect.
**How this changes your future experience:** New users get a meaningfully better cold-start experience the larger the user base grows — but only if the aggregation pipeline is deliberately built; it doesn't happen by default.

**Total Flywheel Score: 14/20**
**Weakest Loop:** Network — the only loop that requires deliberate infrastructure (an opt-in aggregation pipeline) rather than falling out naturally from normal usage.
**Fix for weakest loop:** Design a privacy-preserving, opt-in pipeline from day one that aggregates anonymized correction patterns across users with similar audiograms, so new users get smarter cold-start defaults and the advantage compounds across the whole user base, not just within each individual account.

---

## Encroachment Threat Assessment

### 1. Platform Encroachment
**Attacker:** Apple — specifically the AirPods Pro / Health team.
**Vector:** Apple already shipped a clinical-grade "Hearing Aid" mode in AirPods Pro 2 (iOS 18, 2024) alongside Live Listen and Conversation Boost. The natural next step is layering contextual/adaptive learning onto that existing feature using on-device ML plus Health app data, then bundling it free into an iOS update — no new hardware, no third-party integration needed.
**Time-to-threat:** 12–18 months — Apple has shipped major hearing-health features roughly annually; an adaptive layer fits the established release cadence.
**% of value at risk:** ~60% — but only among iPhone + AirPods Pro users, which skews toward younger/mild-hearing-loss users rather than people on prescription hearing aids from Phonak/Oticon/etc. That segment (~40% of the addressable market) stays out of Apple's reach since those devices don't route through AirPods' pipeline.

### 2. Vertical Competitor
**Attacker:** Whisper (whisper.ai) — an AI-hearing-aid startup whose entire pitch is a continuously learning system ("Whisper Brain") that adapts sound processing from real-world usage. *(Status should be re-verified before relying on this — this space has had funding/ownership changes.)*
**Vector:** Whisper owns the full hardware-to-cloud loop — their own hearing aid device feeds signal-processing telemetry directly into their AI, which is denser and more direct than Wavelength's app-layer nudges on top of third-party OEM hardware.
**Time-to-threat:** Already live — not a future risk, an existing multi-year head start on the data itself.
**% of value at risk:** ~50% — capped by the higher friction of switching hearing aid hardware (vs. switching apps), but a full threat to the "deepest personalization" claim, since their data loop is structurally tighter than ours.

### 3. Adjacent Expansion
**Attacker:** Sonova (parent company of Phonak, the largest hearing aid manufacturer globally) — via the existing myPhonak companion app.
**Vector:** Sonova ships adaptive environment-learning as a routine software update to myPhonak, which millions of existing Phonak wearers already have installed and paired — zero acquisition cost, zero new download.
**Time-to-threat:** 6–12 months — this is a software-only feature add on hardware they already sell, far faster than a hardware refresh cycle.
**% of value at risk:** ~70% within the Phonak installed base specifically (no reason to install a third-party app if it's free and native), but only ~25–30% of the total market since it doesn't touch Oticon, ReSound, Widex, or Signia wearers.

---

## 90-Day Encroachment Plan

*Your partner played the Big Tech attacker. What was their plan to kill you?*

**Attacker:**
**Attack vector (target the weakest loop):**
**Weeks 1-4 - what they ship:**
**Weeks 5-8 - how they poach users:**
**Weeks 9-12 - why users don't come back:**
**Your defense:**
