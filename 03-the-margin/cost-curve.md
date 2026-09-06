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

## Cascading Strategy
<!-- Cheap model → frontier model routing logic -->

**Triage model:** Lightweight, non-LLM environment classifier — handles every classification call by default.
**Frontier model:** Mini LLM (e.g., GPT-4o-mini) — used only for "why did this change?" explanations and medium/low-confidence cases.
**Routing rule:** Triage handles all classification calls; escalate to the frontier model only when confidence is 70–90% (needs softened, explained UI) or the row's Judge Type is `LLM`/`both` per `golden-dataset.md`.
**Expected cascade ratio:** ~90% triage / ~10% frontier (450:50 calls/month).

## Pricing Model

**Current pricing:** N/A — pre-launch prototype, no live pricing yet.
**Proposed AI pricing:** $12/month per user, bundled subscription covering the Leader (adaptive profiles) and Filler ("why did this change?") features; the Caregiver/Family Monitoring Dashboard (Killer) sold as a $5/month add-on.
**Model:** Hybrid — seat-based subscription for core features, usage-gated add-on for the Killer feature.

## Stress Tests

| Scenario | Impact on Margin | Response |
|----------|-----------------|----------|
| Inference costs 3x | AI COGS rises from $2.00 to $6.00/user/month; total COGS (AI + $3 non-AI) rises to $9.00; gross margin drops from ~58% to ~25% at the current $12 price point | Raise the confidence threshold that triggers frontier-model calls (push more volume back to triage), and/or a modest price increase (~$2/month) to hold margin above 40% |
| Heaviest segment doubles | | |
| Model provider raises prices 50% | | |

## Board One-Pager
<!-- Before/After: Old SaaS revenue vs. AI usage revenue for your product -->

**Before (traditional SaaS):**
**After (AI-enabled):**
**Net margin shift:**
