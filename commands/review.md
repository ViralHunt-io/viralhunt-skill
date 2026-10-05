---
description: "One pass of the ViralHunt reviewer: read the content rule from the platform, take the posts in Review that have no verdict on their current content, leave one review per post; never an approval"
argument-hint: "[project name or 'all']"
---

Use the `viralhunt` skill, section 5c ("The reviewer"). Scope: $ARGUMENTS (default: every project).

1. `GET policy.php` and keep the rule in front of you: block list, fix list, scores and thresholds, per-network notes. If it cannot be read, stop and say so; never judge from memory.
2. `GET schedule.php?action=drafts&stage=review&all=1&needs_review=1` (drop `all=1` for one project). If empty, say so in one line and stop.
3. For each post: read the body, the media (or the rendered `design` from `get`), every target network, the time, the first comment and the per-network copies. `media_removed: true` is a `fix` on its own.
4. Judge it against the rule, block first, then fix, then ok, for every target network. Verify claims with `trending.php?source=rss&keyword=` and `search.php?keyword=` before judging them.
5. `POST schedule.php?action=review` once per post: `verdict`, `score`, `scores {tos_risk, rights, fake_news, sensationalism, brand, grammar}`, one warning per issue with the policy's code, the network and the severity, and a note in the post's language that says what to change.
6. Report the answer as it is: the stored `verdict` (the server may have raised yours), `returned` when it went back to Drafts, and `sent` (the send result, or `{skipped, reason}` when a server rule kept it for an owner or admin).
7. Never `update`, `approve` or `cancel`. One line per post at the end.
