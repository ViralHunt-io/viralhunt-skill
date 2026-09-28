---
description: What is viral right now on one network or all of them, for a niche, a keyword or one page; with measured growth and the sample behind it
argument-hint: [network or "all"] [niche, keyword or "page: Name"]
---

Use the `viralhunt` skill, section 2 (Find trending content). The user asked: $ARGUMENTS

1. If a network is named, call `GET trending.php?source=<network>&sort=viral&time_range=7d[&keyword=…][&author=…]` (use `author=` when the user names a page or account). If they said "all" or named none, call `GET search.php?keyword=<the words>` for every network at once, or `trending.php` on the networks they publish to (from `targets`) when there is no keyword.
2. Present the top 8 to 10 as a short ranked list: network, hook of the post, author, the main metric, and `growth_24h` read honestly (`null` means we cannot say, `delta 0` with two readings means flat, quote the real `hours`).
3. Say where the topic lives and where the corpus holds nothing (`sources[].total`).
4. Offer the next step: rework one of them for the user's brand as a draft (section 5b), or find the best time and hashtags for it (section 3).

Free plan: the numbers come back null and the posts are at least 72 hours old; say "numbers are on paid plans", never 0.
