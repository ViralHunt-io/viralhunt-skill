---
description: "Review the drafts waiting in ViralHunt as the team's checker: terms of each network, unverified claims, sensationalism, grammar; one review per draft, never an approval"
argument-hint: "[project name or 'all']"
---

Use the `viralhunt` skill, section 5b ("Reviewing as the team's checker"). Scope: $ARGUMENTS

1. `GET schedule.php?action=drafts` (add `&all=1` for every project). If none, say so and stop.
2. For each draft: read the body, the media (or the rendered `design`), the targets and the existing reviews. Judge the terms of each target network (violence, health claims, politics, minors, copyright, spam patterns), unverified claims (cross-check with `search.php?keyword=` and `trending.php?source=rss&keyword=`), sensationalism and grammar.
3. `POST schedule.php?action=review` once per draft: verdict `ok`, `fix` or `block` (`block` only for what must not go out as it is), `score`, `scores {tos_risk, fake_news, sensationalism, grammar}`, one warning per issue with its network, and a short note on how to fix it.
4. Before giving `ok`, check `auto_approve` on `targets`: when the organization sends drafts by themselves on OK, an `ok` is a publish and you confirm it with the user first.
5. Report a one-line verdict per draft and the Drafts page URL. Do not edit anyone's draft unless asked; never approve.
