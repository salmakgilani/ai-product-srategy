# Data Flywheel Map

> Score each loop 1-5. Your weakest loop is where competitors attack first.
> The four loops below are the M2 starting point - adapt if your product has 2 or 6 loops instead of 4.

## Flywheel Loops

**Product: Signal — AI copilot for PMs**

| Loop | What It Measures | Score 1 | Score 5 | Score |
|------|------------------|---------|---------|-------|
| **Correction** | Do users fix AI outputs? Is that signal captured and reused? | No capture | Automated retraining | 1/5 |
| **Preference** | Does the product learn individual / team preferences over time? | Stateless | Deep personalization | 1/5 |
| **Domain Context** | Does usage in one area improve quality in adjacent areas? | Siloed | Cross-domain transfer | 2/5 |
| **Network** | Does each new user / team make the product better for everyone? | Isolated | Strong network effects | 1/5 |

### Correction Loop - 1/5
**What you capture today:** Nothing persisted. A PM can dismiss, re-title, or edit an opportunity card in the UI, but that correction isn't logged anywhere.
**How it compounds:** Doesn't — each synthesis run starts cold. A card you deleted last week can resurface identically next week.
**How this changes your future experience:** None. Next week's synthesis looks exactly the same whether or not you corrected anything.

### Preference Loop - 1/5
**What you capture today:** Only a binary "Add to Roadmap" click — not stored as a signal about what this PM or team actually values.
**How it compounds:** Not applied. Every PM sees the same ranking regardless of what they've historically prioritized.
**How this changes your future experience:** None. Two PMs on the same feedback pool get identical rankings — Signal doesn't get "more you" over time.

### Domain Context Loop - 2/5
**What you capture today:** A single synthesis run does cross-reference all 6 connected sources at once (e.g., a support-call theme can reinforce a store-review theme in the same cluster).
**How it compounds:** Weak — that blending happens fresh each run. It doesn't persist learned associations, so patterns aren't recognized faster next time.
**How this changes your future experience:** Minor. This week's synthesis is contextually blended, but it isn't sharper than last week's for having seen the pattern before.

### Network Loop - 1/5
**What you capture today:** Nothing shared across tenants — Signal is single-company by design (your Salesforce tickets, your reviews).
**How it compounds:** None. A new customer adopting Signal doesn't improve output quality for any other customer.
**How this changes your future experience:** None. This is a walled garden by construction, not just by omission — likely intentional given data privacy, but worth naming.

**Total Flywheel Score: 5/20**
**Weakest Loop:** Correction — tied lowest with Preference and Network, but it's the highest-leverage fix since Preference learning is structurally downstream of it (you can't learn what a PM values if you never capture what they corrected).
**Fix for weakest loop:** Log every edit, dismiss, and accept action on an opportunity card as a labeled signal, and feed it back into the next clustering + ranking run.

---

## Encroachment Threat Assessment

### 1. Platform Encroachment
**Attacker:**
**Vector:**
**Time-to-threat:**
**% of value at risk:**

### 2. Vertical Competitor
**Attacker:**
**Vector:**
**Time-to-threat:**
**% of value at risk:**

### 3. Adjacent Expansion
**Attacker:**
**Vector:**
**Time-to-threat:**
**% of value at risk:**

---

## 90-Day Encroachment Plan

*Your partner played the Big Tech attacker. What was their plan to kill you?*

**Attacker:**
**Attack vector (target the weakest loop):**
**Weeks 1-4 - what they ship:**
**Weeks 5-8 - how they poach users:**
**Weeks 9-12 - why users don't come back:**
**Your defense:**
