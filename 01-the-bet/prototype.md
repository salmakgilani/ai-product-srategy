# The Prototype Bet

## Prompt Used
```
Build me a web app prototype for an AI CoPilot product called Wavelength.

Who: Hearing aid wearers who want their listening experience to automatically adapt across different environments
Core task: The AI should learn a user's preferred volume/clarity/noise-reduction settings per environment (restaurant, office, outdoors, car) from manual corrections and automatically apply the learned profile next time they're in that environment
First screen: A dashboard showing the currently detected environment, its active sound profile with adjustable sliders, a "learning" status per environment, and a list of previously learned environment profiles with confidence levels (Learning / Tuned / Mastered)
AI moment: User adjusts a slider (volume/clarity/noise-reduction) → AI captures it as a correction, shows a brief "Capturing correction…" state, then updates that environment's profile and confidence level
Output: An updated per-environment sound profile with its new settings, an updated confidence/maturity badge, and a one-line explanation of what changed and why

Clean UI. Dark theme. One page. No login.
```

## What I Built
<!-- One sentence: what does this prototype demonstrate? -->
Wavelength, a context-adaptive listening copilot that learns a hearing aid wearer's preferred sound settings per environment from their own manual corrections, and auto-applies the learned profile the next time they're in a familiar place.

## Tool Used
<!-- v0 / Cursor / Lovable / other -->
Claude (Artifacts)

## Prototype Link
<!-- Paste the shareable URL -->
https://claude.ai/code/artifact/6aad869b-b7d8-428d-9ac1-fdf885ccca69

## AI Value Archetype
<!-- Automator / Copilot / Oracle / Creator / Orchestrator -->
Copilot — it doesn't replace the user's judgment about how they want to hear, it captures every manual correction and quietly applies what it's learned so the user has to make that adjustment fewer and fewer times.

## The Bet in One Sentence
<!-- What you're building, for whom, why now -->
If a listening app can learn a user's per-environment sound preferences from their own corrections and auto-apply them, hearing aid wearers will trust it enough to stop manually re-adjusting every time they enter a familiar place.

## Kill Criteria
<!-- When would you stop? What evidence would kill this bet? -->
If users don't notice or trust the auto-applied changes enough to reduce how often they manually re-adjust — i.e., correction frequency per environment doesn't decline over time — the bet is dead.
