# Kill Switch Audit

**Product: Wavelength — Context-Adaptive Listening Copilot**

## Vendor Dependency Assessment

| Dimension | Current State | Risk Level | 48-Hour Action |
|-----------|--------------|------------|---------------|
| **Provider** | Single vendor — OpenAI (GPT-4o) called directly for both AI features (environment classification, change-explanation); no secondary provider evaluated or under contract. | H | Stand up an Anthropic Claude API account and run the two production prompts (environment-classification, change-explanation) against it in a side-by-side test script — a second live credential and working call path exist by end of day 2. |
| **Abstraction** | OpenAI SDK calls (`openai.chat.completions.create`) are made directly inside feature code (`environmentClassifier.ts`, `profileExplainer.ts`) — no shared interface, no provider-agnostic wrapper. | H | Introduce a single `AIProvider` interface (`generate(prompt, params): Promise<string>`) with an `OpenAIProvider` implementation, and refactor both call sites to go through it — no behavior change, just isolates the vendor SDK behind one seam. |
| **Routing** | 100% of calls go to OpenAI; no fallback, no cost/latency/quality-based selection — a single API outage takes down both AI features at once. | H | Add a failover rule in the `AIProvider` layer: try OpenAI, on error/timeout (>3s) or 5xx, retry once against the Claude provider stood up in step 1, and log which provider actually served each request. |
| **Eval** | No automated tests on AI outputs — quality checked by manually reading sample outputs during development; no regression suite, no fixed test set. | H | Write a 20-example golden-set eval script (`eval/environment-classifier.test.ts`) with expected classifications for known audio-context inputs, run it against both providers, and record pass-rate — this becomes the quality bar any future swap must clear. |

## Portability Score
<!-- Ready / Partial / Locked -->
**Locked** — no abstraction, no routing, no eval today. A pricing change or outage would force an emergency in-code rewrite, not a 48-hour swap.

## If OpenAI doubles pricing tomorrow:
<!-- What's your 48-hour response? -->
Both AI features (environment classification, change-explanation) keep running but at 2x cost with zero mitigation available — no abstraction layer exists to route a percentage of traffic to a cheaper model, so the only 48-hour lever is manually rate-limiting or disabling the explanation feature (the cheaper of the two to cut) while the abstraction/routing work above gets fast-tracked.

## If OpenAI ships a competing product:
<!-- What's defensible that they can't replicate? -->
Wavelength's underlying moat doesn't come from the LLM calls themselves — it's the per-user correction history (Correction/Preference loops, scored 4/5 in the flywheel) that OpenAI has no access to. So a competing OpenAI feature pressures adoption and perception, but doesn't erase Wavelength's actual defensible asset; the real exposure is that today's Locked provider status means Wavelength can't quickly differentiate its AI layer in response.
