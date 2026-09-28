---
description: "Publish now or schedule a post on the user's connected accounts (or leave it as a draft), with per-network copy and the media checked against each network's ceilings"
argument-hint: "<what to post> [networks] [when] ['draft']"
---

Use the `viralhunt` skill, sections 4, 5 and 6. The user asked: $ARGUMENTS

1. `GET schedule.php?action=targets`: the project, its accounts, `needs_reconnect` (name any expired network before anything else) and `media_limits`.
2. Draft the post: the copy in the project's language, a shorter version in `overrides` for Bluesky (300) and X (280) when the body is longer, a first comment if the user gave a link, and the media (check size and length against `media_limits`; say which networks will refuse a video).
3. Show the user exactly what will go out: caption, media, accounts, time. Wait for an explicit yes. If the user did not ask to publish right now, send it with `draft: true` and tell them where it waits.
4. `POST schedule.php?action=create` with the fields above. Report `id`, `status`, and every warning; a `partial` means a network refused and `results[account].error` says why.
5. If the project translates into another one (`translates_to`), offer the translation (section 5b, `action=translate`).
