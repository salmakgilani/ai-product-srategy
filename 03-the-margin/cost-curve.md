# Cost Curve & Pricing Strategy

**Product: Wavelength — Context-Adaptive Listening Copilot**

## Packaging Decision

| Leader | Filler | Killer | Killer usage % | Bundle or add-on |
|---|---|---|---|---|
| Context-adaptive sound profiles — the environment classifier + auto-apply engine; the core AI intelligence and the entire reason someone adopts Wavelength, high willingness-to-pay | "Why did this change?" plain-language explanations — a lightweight LLM call layered on every classification; cheap, seen passively by nearly all users, raises perceived quality without meaningfully raising COGS | Caregiver/Family Monitoring Dashboard — multi-user access controls and notification infra for a linked caregiver to track a user's hearing patterns; heavier backend cost | ~15% | Add-on |

**70% rule check:** Killer usage (~15%) is well under 70%, so it's an add-on, not a bundle inclusion — bundling it would give away margin to the 85% of accounts that would never use it while still absorbing its higher COGS for everyone.

## Cost Model

**Inputs:** ~500 AI requests/user/month · $0.004 blended cost/request · $12 revenue/user/month · $3 non-AI COGS/user/month

| Cost Category | Per-User/Month | Notes |
|--------------|----------------|-------|
| Inference (primary model) | $1.00 | ~50 "why did this change?" explanation calls/month on a mini LLM |
| Inference (cascading/triage) | $0.23 | ~450 environment-classification calls/month on a lightweight, non-LLM classifier |
| Infrastructure | $0.47 | Model serving/orchestration for the classification pipeline |
| Data/storage | $0.20 | Correction logs, per-user profiles, golden-dataset entries |
| Human-in-the-loop | $0.10 | Amortized cost of golden-dataset curation and adversarial-row review |
| **Total AI COGS** | **$2.00** | |

## Cost Curve Detail

| Feature | Complexity | Model Tier | Cost/Req | Volume % | Weighted | Justification | Cost Reduction Lever |
|---|---|---|---|---|---|---|---|
| Environment classification | Simple | Small | $0.0005 | 70% | $0.00035 | Bounded categorical decision from sensor data, no language reasoning needed | Debounce signal — reclassify only after a stable 5–10s read |
| Weekly listening-insight summary | Medium | Mid | $0.006 | 20% | $0.0012 | Short language synthesis over one user's weekly log — bounded scope | Skip regeneration when the week's pattern is unchanged |
| Ambiguous/conflicting-signal resolution | Complex | Frontier | $0.025 | 10% | $0.0025 | Needs real judgment under ambiguity (conflicting or novel signals) | Mid-tier model attempts first; escalate to Frontier only if Mid reports low confidence |
| **Blended** | | | | 100% | **~$0.004** | | |

## Cascading Strategy
<!-- Cheap model → frontier model routing logic -->

**Triage model:** Small-tier classifier — handles all classification calls.
**Frontier model:** Frontier-tier model — used only when Mid reports low confidence on ambiguous/conflicting cases.
**Routing rule:** Small handles all classification → weekly insights go to Mid → ambiguous cases try Mid first, escalate to Frontier only on low confidence.
**Expected cascade ratio:** 70% Small / 20% Mid / 10% Frontier.

## Pricing Model

**Current pricing:** N/A — pre-launch prototype.
**Proposed AI pricing:** Base $6/mo + $0.015 per auto-applied environment switch (~360/mo for an active user, ≈$11.40/mo blended). Caregiver Dashboard (Killer) remains a $5/mo add-on.
**Model:** Hybrid — base fee + usage.
**Strategy:** Penetrate — the moat depends on correction-data volume compounding before OEMs/Apple catch up (6–18 month window), so speed of adoption matters more than early margin.
**Unit of work:** Auto-applied environment switches — ties price to the actual promise (fewer manual corrections over time), not just app access.
**Labor test:** Doesn't map to an hourly-wage comparison; the real avoided cost is hearing aid abandonment, not billable time.
**Proof needed:** Pilot data showing manual-correction frequency declining over the first 4–6 weeks — not yet collected.

## Stress Tests

| Scenario | Impact on Margin | Response |
|----------|-----------------|----------|
| Inference costs 3x | AI COGS rises from $2.00 to $6.00/user/month; total COGS (AI + $3 non-AI) rises to $9.00; gross margin drops from ~58% to ~25% at the current $12 price point | Raise the confidence threshold that triggers frontier-model calls (push more volume back to triage), and/or a modest price increase (~$2/month) to hold margin above 40% |
| Heaviest segment doubles | | |
| Model provider raises prices 50% | | |

## Board One-Pager
<!-- Before/After: Old SaaS revenue vs. AI usage revenue for your product -->

**Before (traditional SaaS):** Static, manual-presets app. Revenue: $8/seat × 10,000 seats = $80,000/mo. COGS: $20,000/mo (fixed). Gross margin: 75%.
**After (AI-enabled):** Revenue: $0.015/switch × 3.6M switches/mo + $60,000 base = $114,000/mo. COGS: $50,000/mo (variable, scales with usage). Gross margin: 56%.
**Net margin shift:** 75% → 56%. Margin % drops because AI inference cost scales with usage instead of being near-fixed — but revenue is up 42.5% and gross profit is still slightly higher in dollars ($60K → $64K). The real case: usage-based pricing creates organic NRR expansion (more learned environments → more revenue per account) that a flat-seat price can't generate. Margin % alone is the wrong metric here.
