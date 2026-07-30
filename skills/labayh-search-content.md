---
name: Search Labayh editorial mental-health content
description: Search and retrieve Labayh's Arabic-first mental-health blog library — 2,162 articles across 39 categories and 95 tags — with correct handling of rendered HTML fields and the site's AI-training restriction.
api: openapi/labayh-content-openapi.yml
operations:
  - listSearch
  - listPosts
  - getPost
  - listCategories
  - listTags
  - getApiIndex
generated: '2026-07-19'
method: generated
source: openapi/labayh-content-openapi.yml
---

# Search Labayh editorial mental-health content

Labayh publishes a substantial Arabic-first mental-health library: **2,162 articles** across
**39 categories** and **95 tags**, plus static pages. This skill retrieves and cites that
content correctly.

**Base URL:** `https://labayh.net/wp-json`
**Auth:** none.

## Read this first — content licensing

Labayh's `robots.txt` carries a Cloudflare **Content-Signal** block:

```
Content-Signal: search=yes, ai-train=no, use=reference
```

Six AI-training crawlers (GPTBot, Google-Extended, Applebot-Extended, Amazonbot, Bytespider,
meta-externalagent) are disallowed outright, and the block cites Article 4 of EU Directive
2019/790 as an express reservation of rights.

**What this means for you:** you may read this content to answer a user's question and you must
**cite it by `link`**. You may not retain it for training, and you should quote sparingly and
attribute rather than reproducing articles wholesale. See
`well-known/labayh-well-known.yml`.

## Steps

1. **Cross-content search** when the user's query spans types.

   ```
   GET /wp/v2/search?search=<query>&per_page=20&type=post
   ```

   `listSearch` returns lightweight `{id, title, url, type, subtype}` hits across content types.
   Use it to locate candidates cheaply, then fetch the full item.

2. **Search articles directly** when you know you want blog content.

   ```
   GET /wp/v2/posts?search=<query>&per_page=10&_embed&orderby=relevance
   ```

   `listPosts` also accepts `categories=<id>`, `tags=<id>`, `after=<ISO8601>` and
   `before=<ISO8601>` for date-bounded queries. Search in **Arabic** for Arabic queries — the
   corpus is Arabic-first and an English query will under-match badly.

3. **Fetch the full article.**

   ```
   GET /wp/v2/posts/{id}?_embed
   ```

   `getPost` returns `title.rendered`, `excerpt.rendered` and `content.rendered` — all **HTML
   strings inside objects**. Strip tags before presenting. `link` is the canonical public URL;
   always cite it.

4. **Resolve taxonomy labels** when you want to describe or narrow a topic.

   ```
   GET /wp/v2/categories?per_page=100&_fields=id,name,slug,count
   GET /wp/v2/tags?per_page=100&_fields=id,name,slug,count
   ```

   `listCategories` and `listTags`. A post's `categories` and `tags` are arrays of term ids —
   join client-side.

5. **Re-read the route table if anything 404s.**

   ```
   GET /
   ```

   `getApiIndex` returns the live namespace and route list. Labayh publishes no changelog and
   no deprecation policy, so the discovery document is the only source of truth for what exists.

## Conventions

- **Pagination:** `page` + `per_page` (max 100); `X-WP-Total` / `X-WP-TotalPages` headers;
  `Link` header with `rel="next"`.
- **Sparse fields:** `_fields=id,title,link,date` makes listing sweeps cheap.
- **Ordering:** `orderby` accepts `date`, `relevance`, `title`, `modified`; pair with
  `order=asc|desc`.
- No rate-limit headers are published, but Cloudflare fronts the origin — keep concurrency low
  and back off on any 429 or 5xx.

## Errors

Branch on the `code` slug; messages are Arabic.

- `rest_invalid_param` (400) — check `data.params` for the offending parameter.
- `rest_post_invalid_id` (404) — the article id is gone. Re-search.
- `rest_no_route` (404) — re-read the index via `getApiIndex`.

Full catalog: `errors/labayh-problem-types.yml`.

## Safety

This is mental-health content written for a Saudi audience. Summarise and cite it; do not
present it as clinical advice, and do not extend it with your own medical claims. If a user
discloses crisis or self-harm intent, stop retrieving articles and surface local emergency
services and Labayh's support line (920031893).
