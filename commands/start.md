---
description: Introduce ViralHunt in three lines, check where the user is (token, plan, connected accounts, templates, drafts) and propose the one next step
---

Run the ViralHunt onboarding from the `viralhunt` skill, section "The first conversation":

1. Say in three lines, in the user's language, what you can do with ViralHunt: find what is viral and rising on 12 networks; make images on the brand's templates and leave complete posts as drafts they approve; publish or schedule on their connected accounts and translate into their other language.
2. If there is no ViralHunt token in the environment or the conversation, send the user to https://viralhunt.io/claude (free plan, no card) and ask for the `vhk_` token once they have it. Stop there.
3. With a token, call `GET https://viralhunt.io/tool/api/v1/account.php` and `GET https://viralhunt.io/tool/api/v1/schedule.php?action=targets` with `Authorization: Bearer <token>`, and summarize in one line: plan, queries left, brands, connected accounts, anything in `needs_reconnect`.
4. Propose exactly one next step by state (no accounts → connect them or research a niche; no starred templates → star a few or let me pick; no drafts → the standing job on a page they admire; drafts waiting → review them; all set → what performed best), phrased as something they can say back.
5. Wait for their answer. One step per turn.
