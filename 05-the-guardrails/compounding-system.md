# Compounding System Design

**Product: Wavelength — Context-Adaptive Listening Copilot**

## Feedback Loops

| Loop | Input | Output | Compounds? | Status |
|------|-------|--------|-----------|--------|
| Recursive Learning | Manual volume/clarity/noise corrections per environment | Updated per-environment profile + confidence level | Y | active |
| Cross-Domain Transfer | Learned response to one environment's acoustic signature | Starting-point guess for a similar new environment | N | missing |
| Network Intelligence | Anonymized, opted-in corrections across similar users | Smarter cold-start defaults for new users | Y | missing |

**Broken loop identified by partner:** Corrections Transfer — documented as active ("Y" in golden-dataset.md's User Control Surface) but not actually implemented; the Kill Switch audit independently confirms Portability Score: Locked, with no correction-to-retraining pipeline built. The loop is theoretical, not real, and the two documents contradict each other. Audit also surfaced a Samsung-path gap: no data-minimization/redaction step before location and hearing-profile context reaches third-party frontier model providers.
**Fix plan:** (1) Downgrade the golden-dataset.md claim from "Y" to "Planned — 48hr action item" so no document overstates a capability that doesn't exist yet. (2) Build the actual correction-logging pipeline per the existing Kill Switch 48-hour action before re-marking it "Y." (3) Add a data-minimization/redaction step to Governance Policy's Required Controls, stripping location and hearing-profile identifiers before any request reaches a third-party frontier model.

## Frozen-Model Stress Test
*If the foundation model doesn't improve for 3 months and competitors have identical access — does Wavelength still improve?*

Yes — Recursive Learning doesn't depend on the frontier model; it compounds purely from accumulating user corrections, and the frontier model only handles ~10% of calls (ambiguous-signal resolution, explanations). Additional complexity: freezing the model commoditizes that 10% slice completely, since every competitor gets identical output from it. The only non-commoditized asset left is proprietary correction data — which makes Network Intelligence (currently missing) the real vulnerability: a competitor with the same frozen model could still win on cold-start quality by building the cross-user data pipeline first.

## Context Connectivity
<!-- How does knowledge flow across teams and domains? Where does it silo? -->
Correction data lives per-user in logs but isn't wired to the places that should share it: the classifier's own retraining pipeline, the Cross-Domain Transfer layer, and other users' models. Three disconnected silos today, each already scoped as a future fix elsewhere (kill-switch, flywheel) but not yet built.

## Governance Policy

**Scope:** Covers the environment classifier, the Small→Mid→Frontier model cascade (explanations, ambiguous-signal resolution), the correction/golden-dataset pipeline, and the not-yet-built Network Intelligence aggregation layer. Data in scope: acoustic/environmental sensor signals, GPS-derived location context, per-user correction logs, hearing-profile data. Users in scope: primary hearing aid wearers and linked caregivers. Explicitly out of scope: the hearing aid firmware/DSP itself, which stays owned and regulated by the OEM (Phonak/Oticon/etc.) — Wavelength only orchestrates via the companion app layer.

**Autonomy boundaries:** OK solo — auto-applying a profile at >90% confidence, routine classifier inference, routine weekly insight generation. Needs human review (async) — any new classifier model version must clear the golden-dataset bar (90% accuracy, <2% hallucination) and get sign-off from the on-call PM/eng owner before deploy. Always human (blocking) — auto-applying below the 70% confidence floor without the human-in-loop prompt is a policy violation; any Network Intelligence launch requires legal/privacy sign-off first; caregiver dashboard access is only ever granted by explicit primary-user consent, never auto-granted by the AI.

**Escalation triggers:** Hallucination rate >3% → pages on-call PM, auto-rollback to confirm-before-apply. Drift velocity >1%/wk → triggers gold-set audit. Same "Unknown"/out-of-taxonomy environment flagged 3+ times for one user → escalate to human review of whether the taxonomy needs expanding. Any signal suggesting a user's hearing loss is worsening rapidly → routed to a human-reviewed check-in, never acted on autonomously — Wavelength isn't a licensed medical device and shouldn't make clinical inferences on its own.

**Audit cadence:** Real-time — latency p95, hallucination rate (Datadog). Weekly — golden-dataset accuracy run + gold-set audit trigger review. Monthly — escalation-trigger log review (how often human-in-loop fired, and why) + confidence-threshold calibration check. Quarterly — full governance policy review, regulatory-exposure reassessment, and a red-team pass on the adversarial rows in the golden dataset.

**Regulatory exposure (EU AI Act / other):** EU AI Act — likely "limited risk" (transparency obligations) rather than "high risk," since Wavelength personalizes an existing prescribed device rather than diagnosing or treating, but this is a genuinely nuanced boundary (it does autonomously adjust a health device's settings) and needs actual legal review, not an assumption. GDPR — audio/location/hearing data is sensitive; Network Intelligence's cross-user data sharing must be anonymized and explicitly opt-in under Article 9 (special category health data). Sector-specific — if Wavelength ever integrates directly with hearing aid firmware rather than staying at the companion-app layer, FDA/medical-device rules could apply (see Apple's AirPods Pro Hearing Aid FDA clearance as precedent) — flagged as a line not to cross without separate regulatory review. Required controls: encryption at rest/in transit, audit logs on every auto-applied profile change, the human-in-loop path already built via Confidence UX, and a legal gate before Network Intelligence ships.

## Agent Topology
<!-- If using agents: what can each agent do? What can't it do? Who approves what? -->

## Shadow AI Audit

| Tool | Owner | Risk Level | Decision |
|------|-------|-----------|----------|
| | | H / M / L | keep / govern / kill |
| | | H / M / L | keep / govern / kill |
| | | H / M / L | keep / govern / kill |

**Total tools found:**
**Tools after triage:**
**Estimated hidden spend:**
