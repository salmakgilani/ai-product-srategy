# Three-Axis Vulnerability Diagnostic

## Product
<!-- Name the product you're diagnosing. Real product at your company — not a hypothetical. -->

**Product:** Signal — AI copilot for PMs
**Your Role:** Product Manager, building this as a prototype bet

---

## Scores

### Contextual Moat — 2/5
*Workflow depth × switching cost. Would users leave in a weekend if a competitor showed up?*

**Score rationale:**
Signal is currently a read-only synthesis layer, not a system of record. PMs still make and track roadmap decisions in Jira/Linear/Productboard — "Add to Roadmap" pushes data out, it doesn't hold it. If a PM stopped using Signal tomorrow, nothing breaks; they just go back to reading six tabs. No workflow gravity yet, and low switching cost.

**Named attacker (from partner challenge):**
Productboard — already owns the feedback-to-roadmap workflow PMs use daily; could ship AI clustering as a feature update inside a tool teams are already locked into, rather than asking them to add a new one.

---

### Data Advantage — 2/5
*Proprietary signal that compounds with usage. What do you see that OpenAI doesn't?*

**Score rationale:**
All six inputs (Salesforce, store reviews, support calls, interviews, marketing, sales) are the customer's own data, pulled via standard APIs — nothing proprietary to Signal. There's no outcome-tracking loop yet (e.g., "this cluster got added to roadmap → shipped → did retention move") that would let the model compound advantage over time. Today it's a one-shot transformation, not a learning system.

**Named attacker (from partner challenge):**
A thin LLM wrapper — anyone with the same Salesforce/Gong/Zendesk API keys and a clustering prompt can reproduce the core output in a weekend.

---

### Platform Exposure — 1/5
*Encroachment risk × pivot speed. If Apple/Google/OpenAI ships your hero feature native — then what?*

**Score rationale:**
This is the sharpest risk. Salesforce is literally one of Signal's own six source integrations — meaning the platform Signal depends on for data already has native access, an existing PM-adjacent customer base, and is actively shipping agentic AI (Agentforce/Einstein Copilot) directly into that data. They need zero new integrations to ship "feedback synthesis" as a bolt-on feature.

**Named attacker (from partner challenge):**
Salesforce (Agentforce / Einstein Copilot).

---

## Top Vulnerability
<!-- One line: what's the single biggest strategic risk? -->
Platform exposure — the source-of-truth platform Signal depends on for its core data can ship the same synthesis natively, with zero integration friction, eliminating the reason for a separate layer to exist.

## Confidence Level
<!-- H / M / L — how confident are you in this bet after the diagnostic? -->
H — this isn't speculative; it's the most structurally obvious risk since the platform already owns the data Signal is built on top of.
