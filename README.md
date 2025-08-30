# Money Moves Daily — Automated Newsletter + Social Repurposer

This is a **turnkey starter kit** for an automated content business:
- Curates finance/business headlines from RSS feeds
- Uses an LLM to draft a daily newsletter + blog post
- Generates social captions (X/LinkedIn/IG carousel script)
- Builds and publishes a static site + RSS feed (for Zapier/Make to pick up)
- (Optional) Auto-posts to social via Buffer API
- Schedules itself daily with GitHub Actions

> **Your daily time:** ~15–30 min to skim the draft (or set to fully auto-send).

---

## Quick Start

1. **Create a new GitHub repo** (public or private) and upload this folder.
2. **Enable GitHub Pages** in your repo settings (Source: `Deploy from a branch`, Branch: `main`, Folder: `/site`).  
   The site will host your **RSS feed** at `https://<your-username>.github.io/<your-repo>/index.xml`.
3. **Add Secrets** in your repo (Settings → Secrets and variables → Actions → New repository secret):
   - `OPENAI_API_KEY` — for content generation.
   - *(Optional)* `BUFFER_ACCESS_TOKEN` — if you want auto-posting via Buffer.
4. **Edit** `config.yaml` to your liking (niche, tone, sources, affiliate links).
5. Commit + push. GitHub Actions will run **daily at 8:00 AM America/Indiana/Indianapolis** by default.

---

## Automations Overview

- **Content gen**: `newsletter_bot/main.py` fetches RSS items, asks the LLM to produce a structured newsletter, renders HTML/Markdown, and writes to `/site/posts/...` and `/out`.
- **RSS feed**: `newsletter_bot/build_rss.py` compiles `/site/posts` into `/site/index.xml`.
- **Social**: `newsletter_bot/social_templates.py` writes `out/social/` (thread, caption, carousel script).  
  To auto-post, the workflow calls `newsletter_bot/platforms/buffer.py` if `BUFFER_ACCESS_TOKEN` is set.
- **Scheduling**: `.github/workflows/daily.yml` runs everything daily and commits results to `main`.

---

## Email Sending (Beehiiv/Substack/Mailchimp/ConvertKit)

This kit outputs a **polished HTML/Markdown post + RSS**. To auto-send email:
- Use **Zapier** or **Make**: *RSS (new item) → Create Draft Post (Beehiiv/Substack) → Publish/Send*.
- Or paste the HTML from `/out/latest.html` into your email platform and schedule there.
- You can also host on **Buttondown** or similar via their API (map `/out/latest.md`).

> **Tip:** Start with a weekly cadence, then go daily as you grow.

---

## Branding & Monetization

- **Logo**: `brand/logo.svg` (editable SVG)
- **Palette & typography**: `brand/brand.json`
- **Affiliate links**: Configure `config.yaml::affiliate_links`. The generator inserts links contextually.

---

## Local Development

```bash
# Python 3.10+ recommended
pip install -r requirements.txt

# Generate a draft
export OPENAI_API_KEY=sk-...
python newsletter_bot/main.py
python newsletter_bot/build_rss.py
```

Outputs land in:
- `/out/latest.html` (email/blog HTML)
- `/out/latest.md` (Markdown)
- `/out/social/*` (X thread, LinkedIn, IG carousel script)
- `/site/posts/*` (published post files)
- `/site/index.xml` (RSS)

---

## Notes & Disclaimers

- This kit is for **educational/informational** content. It is **not financial advice**.
- Respect each platform’s API & automation rules.
- You are responsible for compliance with CAN-SPAM/GDPR and affiliate disclosures.
- The LLM prompts are engineered for short, scannable content — feel free to tune!

Enjoy! 🚀
