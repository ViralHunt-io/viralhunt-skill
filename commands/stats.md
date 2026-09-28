---
description: "What performed: the networks' own engagement of the posts published through ViralHunt, by network and by brand, and more content like the winners"
argument-hint: "[project] [days, default 30]"
---

Use the `viralhunt` skill, section 8a. The user asked: $ARGUMENTS

1. `GET stats.php[?project=…][&days=30]`. If `ready: false`, say no stats have been collected yet and stop.
2. Summarize in a few lines: the network that performed best, the brand's totals, the top three posts with their permalinks. Numbers only from the response; never invent one.
3. Read the winning topics off the top posts and run `GET search.php?keyword=<topic>` to propose two or three pieces of content like them, each as something the user can turn into a draft (section 5b).
