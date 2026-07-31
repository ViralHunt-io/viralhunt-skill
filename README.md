# ViralHunt Skill

An Agent Skill that lets any AI agent **find what's trending across social networks and
schedule/publish posts** to your connected accounts — powered by the [ViralHunt.io](https://viralhunt.io) API.

ViralHunt is a trending-content radar + cross-network scheduler: it tracks what's gaining
velocity on TikTok, Instagram, X, Facebook, Pinterest and Reddit, you curate it, and
publish everywhere from one place. A **BuzzSumo alternative** at a fraction of the price,
with an API that agents can drive end to end.

## What the skill can do

- **Discover** trending/viral posts by network, niche and time window (`trending.php`)
- **List** the projects/brands and connected accounts you can post to (`schedule.php?action=targets`)
- **Publish now or schedule** a post (text + media) across accounts (`schedule.php?action=create`)
- **Upload** an image/video and get a public URL (`schedule.php?action=upload`)
- **Check status** of any post it created (`schedule.php?action=get`)
- Optionally organize work on the editorial **kanban board** (`cards.php`, `my-cards.php`)

## Get a token

1. Sign up at **https://viralhunt.io** (free 7-day trial, no card).
2. Go to **Account → API Access** and create a personal token (`vhk_…`).
3. Give the token to your agent. It's sent as `Authorization: Bearer vhk_…`.

Full API docs: **https://viralhunt.io/api**

## Install

### Claude Code
Add the marketplace, then install the plugin:

```
/plugin marketplace add viralhunt-io/viralhunt-skill
/plugin install viralhunt
```

### Any agent (manual)
Copy `skills/viralhunt/SKILL.md` into your agent's skills folder, or paste its contents as
a system/skill instruction. The whole skill is self-contained in that one file.

## License

MIT. ViralHunt is a product of viralhunt.io.
