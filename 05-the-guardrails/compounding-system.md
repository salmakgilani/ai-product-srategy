# Compounding System Design

**Product: Wavelength — Context-Adaptive Listening Copilot**

## Feedback Loops

| Loop | Input | Output | Compounds? | Status |
|------|-------|--------|-----------|--------|
| Recursive Learning | Manual volume/clarity/noise corrections per environment | Updated per-environment profile + confidence level | Y | active |
| Cross-Domain Transfer | Learned response to one environment's acoustic signature | Starting-point guess for a similar new environment | N | missing |
| Network Intelligence | Anonymized, opted-in corrections across similar users | Smarter cold-start defaults for new users | Y | missing |

**Broken loop identified by partner:**
**Fix plan:**

## Frozen-Model Stress Test
*If the foundation model doesn't improve for 3 months and competitors have identical access — does Wavelength still improve?*

Yes — Recursive Learning doesn't depend on the frontier model; it compounds purely from accumulating user corrections, and the frontier model only handles ~10% of calls (ambiguous-signal resolution, explanations). Additional complexity: freezing the model commoditizes that 10% slice completely, since every competitor gets identical output from it. The only non-commoditized asset left is proprietary correction data — which makes Network Intelligence (currently missing) the real vulnerability: a competitor with the same frozen model could still win on cold-start quality by building the cross-user data pipeline first.

## Context Connectivity
<!-- How does knowledge flow across teams and domains? Where does it silo? -->
Correction data lives per-user in logs but isn't wired to the places that should share it: the classifier's own retraining pipeline, the Cross-Domain Transfer layer, and other users' models. Three disconnected silos today, each already scoped as a future fix elsewhere (kill-switch, flywheel) but not yet built.

## Governance Policy

**Scope:**
**Autonomy boundaries:**
**Escalation triggers:**
**Audit cadence:**
**Regulatory exposure (EU AI Act / other):**

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
