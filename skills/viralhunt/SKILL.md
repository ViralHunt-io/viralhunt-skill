---
name: viralhunt
description: >-
  Discover what's trending/going viral across TikTok, Instagram, X, Facebook,
  Pinterest and Reddit, and schedule or publish posts to the user's own connected
  social accounts — powered by the ViralHunt.io API. Use this whenever the user
  wants to find viral or trending content in a niche, research what's performing
  on social right now, or draft/schedule/publish social posts across networks.
license: MIT
metadata:
  author: viralhunt-io
---

# ViralHunt

ViralHunt (https://viralhunt.io) is a trending-content radar + cross-network scheduler.
This skill lets you (an agent) run the full loop for the user: **find what's going viral →
curate it → schedule/publish it** to their connected social accounts.

## 1. Get an API token (once)

Every call needs a personal token. The user creates one at
**https://viralhunt.io → Account → API Access** (owner/admin), then gives it to you. It
looks like `vhk_...`. If the user doesn't have one yet, send them there — a free 7-day
trial gives them a working key.

Send it on every request:

```
Authorization: Bearer vhk_the_users_token
```

Base URL: `https://viralhunt.io/tool/api/v1/`
Rate limit: 100 requests/hour per token (headers `X-RateLimit-Remaining` / `-Reset`; a 429
means wait until the reset). All responses are JSON: `{"success":true,"data":{…}}` or
`{"success":false,"error":{"code","message"}}`.

## 2. Find trending content

`GET trending.php?source=<network>&sort=<sort>&time_range=<range>`

- `source`: `tiktok` | `instagram` | `x` | `facebook` | `pinterest` | `rss`
- `sort`: `viral` (default), plus per-network options like `most_liked`, `most_viewed`,
  `most_commented`, `newest`
- `time_range`: `24h` | `7d` | `30d` | `3m` | `all`
- optional: `keyword=...`, `min_engagement=N`, `page=N`, `per_page=N` (max 100)

```bash
curl -H "Authorization: Bearer $VH" \
  "https://viralhunt.io/tool/api/v1/trending.php?source=tiktok&sort=viral&time_range=7d&per_page=10"
```

Returns ranked posts with engagement metrics, author, URL and thumbnail. Use this to tell
the user what's gaining velocity, or to pick something to curate and repost.

## 3. See where you can publish

`GET schedule.php?action=targets`

Returns the user's **projects** (brands/workspaces) and the **connected accounts** you can
post to in each:

```json
{ "project": {"id":1,"name":"My Brand","timezone":"America/Mexico_City"},
  "projects": [ {"id":1,"name":"My Brand","networks":["instagram","facebook"],"account_count":4} ],
  "accounts": [ {"account_id":12,"network":"instagram","name":"@mybrand"} ] }
```

If a project has **0 accounts**, the user is on a plan without connected accounts —
publishing won't work until they connect accounts in the app (Agency plans).

## 4. Publish now or schedule

`POST schedule.php?action=create` with a JSON body:

```json
{
  "project": "My Brand",              // or "project_id": 1 — REQUIRED if the org has >1 project
  "body": "Your caption text",
  "media": ["https://.../image_or_video.mp4"],   // public URLs; optional if body is set
  "target_account_ids": [12, 15],     // omit = ALL accounts in the project
  "networks": ["instagram","facebook"],// alternative to target_account_ids
  "scheduled_at": "2026-08-01T15:30:00Z", // ISO-8601 UTC; omit = publish immediately
  "first_comment": "Link in comments 👇"  // optional; posted as the first comment
}
```

```bash
curl -X POST -H "Authorization: Bearer $VH" -H "Content-Type: application/json" \
  -d '{"project":"My Brand","body":"Hello world","networks":["instagram"]}' \
  "https://viralhunt.io/tool/api/v1/schedule.php?action=create"
```

Returns `{ id, status, scheduled_at, targets, results, warnings }`. `status` is
`processing` (publishing now), `scheduled` (queued), `partial` (some targets failed — see
`warnings`), or `failed`. It fans out one post per target account.

**Rules to follow so you don't create bad posts:**
- If the org has more than one project, you **must** pass `project` or `project_id` — the
  API refuses to guess (so a post never lands on the wrong brand).
- Only pass `scheduled_at` in the future, in UTC.
- Verify the content before publishing: don't repost fake news, copyrighted media, or spam
  — that gets the user's accounts banned. When unsure, show the user and ask.

## 5. Upload media (optional)

If you have a local file instead of a URL:

`POST schedule.php?action=upload` — multipart form field `file` (jpg/png/gif/webp/mp4/mov,
≤50MB). Returns `{ "url": "https://..." }`. Pass that URL in `media` on create.

## 6. Edit or cancel a scheduled post

Posts can be edited ONLY while their status is `scheduled` (not yet publishing), and PostProxy
won't allow an edit less than ~5 minutes before publish time.

- **Cancel:** `POST schedule.php?action=cancel` — `{"id": 123}`. Cancels the not-yet-published
  targets. If everything already published you get `409 already_published`.
- **Update:** `POST schedule.php?action=update` — `{"id": 123, "body": "...", "media": ["..."],
  "networks": ["facebook"], "scheduled_at": "2026-08-01T15:30:00Z"}`. Only the fields you send
  change. The project cannot be changed.

Agent rules:
- Before editing, `GET schedule.php?action=get&id=123` and confirm status is `scheduled`.
- On `409` (already publishing/published or too close to publish time), re-fetch and tell the
  user — never retry an update blindly.
- After any edit, `GET` again and confirm the change landed before reporting done.

## 7. Check status

`GET schedule.php?action=get&id=<post_id>` → the post's current status and per-network
results. Call `POST schedule.php?action=sync` first to refresh from the networks.

## Editorial board (optional)

You can also organize work on the user's kanban board instead of publishing directly:
`GET columns.php`, `GET my-cards.php` (cards assigned to you), `POST cards.php` (create a
card), `GET/POST comments.php`. See the full docs at **https://viralhunt.io/api**.

## Error handling

| HTTP | code | what to do |
|------|------|-----------|
| 401 | `invalid_key` | token missing/revoked — ask the user for a fresh one |
| 403 | `no_accounts` | project has no connected accounts — user must connect them in-app |
| 422 | `project_required` | org has multiple projects — pass `project`/`project_id` |
| 422 | `validation_error` | fix the parameter named in the message |
| 429 | `rate_limited` | wait until `X-RateLimit-Reset`, then retry |
| 503 | `scheduler_unavailable` / `publishing_unavailable` | scheduling not enabled on this site |

Always surface the `error.message` to the user verbatim — it explains exactly what to fix.
