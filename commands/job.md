---
description: "The standing job on one page or account: take its most viral posts, verify each, rewrite for the brand, make the image on a template, leave drafts a few a day"
argument-hint: "<network> <page or account name> [period, e.g. 1y] [drafts per day]"
---

Run the standing job from the `viralhunt` skill, section 5b ("A standing job, one post at a time"). The user asked: $ARGUMENTS

Before starting, confirm in one message: the project (from `targets`), the page, the period, how many drafts a day (default five), and that everything goes to Drafts for the user to approve. Then:

1. `GET trending.php?source=<network>&author=<page>&time_range=<period>&sort=viral&per_page=30`. Keep the list and work it in order, one post per turn.
2. For each post: verify the claim (`search.php?keyword=` and `trending.php?source=rss&keyword=`; a DOI comes from the paper found by title, never invented). Skip what does not hold up and say why in one line.
3. Rewrite in the project's language and the brand's voice; never copy the source text.
4. Pick a template (`templates.php?favorites=1`, then by `suits` and `instructions`), fill only its dynamic variables; the source photo goes in `image` only if it is unbranded, otherwise a licensed picture with its credit.
5. `schedule.php?action=create` with `draft: true`, the `design` (or your rendered PNG in `media`), the copy in `body`, `scheduled_at` spread at the project's best hours (section 3). Then `action=review` on it with the source URL and what you verified.
6. Report after each post; at the end give the Drafts page URL. Never approve.

Read `schedule.php?action=edit_log&since=<start of this batch minus 30 days>` first and apply what people changed last time.
