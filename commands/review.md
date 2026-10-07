---
description: "One pass of the ViralHunt reviewer: read the content rule from the platform, take the posts in Review that have no verdict on their current content, leave one review per post; never an approval"
argument-hint: "[project name or 'all']"
---

Use the `viralhunt` skill, section 5c ("The reviewer"). Scope: $ARGUMENTS (default: every project).

1. `GET policy.php` and keep the rule in front of you: block list, fix list, scores and thresholds, per-network notes. If it cannot be read, stop and say so; never judge from memory.
2. `GET schedule.php?action=drafts&stage=review&all=1&needs_review=1` (drop `all=1` for one project). If empty, say so in one line and stop.
3. For each post: read the body, the media (or the rendered `design` from `get`), every target network, the time, the first comment and the per-network copies. `media_removed: true` is a `fix` on its own.
4. Judge it against the rule, block first, then fix, then ok, for every target network. Verify claims with `trending.php?source=rss&keyword=` and `search.php?keyword=` before judging them.
5. First decide what the post IS (the policy's `kinds`: fact, meme, quote, opinion, promo) and apply only that kind's rules: a meme, a joke, a quote or an opinion gets no source, DOI or fake-news reading. Then, on every post: the picture against the text (`image_match` 0-100, a mismatch is `fix_image_mismatch`), spelling and grammar in the post's language, how likely an AI wrote it (`ai_written` 0-100, said in the note), and the facts, with the DOI rule (a scientific, medical or statistical claim without a DOI or a primary source is `fix_doi`: the author is flagged). Then one overall `risk10`, 1 (safe) to 10 (breaks a network's terms).
6. `POST schedule.php?action=review` once per post: `verdict`, `score`, `scores {tos_risk, rights, fake_news, sensationalism, brand, grammar, risk10, image_match, ai_written}`, one warning per issue with the policy's code, the network and the severity, a note in the post's language that says what to change, and `content_hash` as the row carried it. A `409 content_changed` means the post was edited meanwhile: read it again, review the current content.
7. Report the answer as it is: the stored `verdict` (the server may have raised yours), `returned` when it went back to Drafts, and `sent` (the send result, or `{skipped, reason}` when a server rule kept it for an owner or admin).
8. Never `update`, `approve` or `cancel`. One line per post at the end (id, verdict, risk10, reason).
