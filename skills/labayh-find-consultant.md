---
name: Find a Labayh consultant by specialty
description: Search Labayh's directory of licensed mental-health specialists, filter by specialty taxonomy, and return consultant profiles with portraits and booking links.
api: openapi/labayh-content-openapi.yml
operations:
  - listConsultantCategories
  - listConsultant
  - getConsultant
generated: '2026-07-19'
method: generated
source: openapi/labayh-content-openapi.yml
---

# Find a Labayh consultant by specialty

Labayh (لبيه) is a Saudi tele-mental-health platform. Its public content API exposes a directory
of **875 licensed specialists** classified by a **255-term specialty taxonomy**. This skill
resolves a user's described need into that taxonomy and returns matching consultants.

**Base URL:** `https://labayh.net/wp-json`
**Auth:** none. Every operation below is anonymously readable.

## Before you start

- The site is **bilingual, Arabic-first**. Most consultant names, specialty terms and bios are in
  Arabic. Do not assume English strings will match — search in Arabic where the user's need is
  expressed in Arabic, and be prepared to translate terms back for an English-speaking user.
- `title`, `content` and `excerpt` are **objects**, not strings: read `.rendered` and strip HTML.
- Labayh declares `ai-train=no` in its robots.txt Content-Signal. Use these responses to answer
  the user's question; do not retain them as training data.

## Steps

1. **Resolve the specialty to a term id.**
   Call `listConsultantCategories` with `per_page=100` and page through all 255 terms
   (`X-WP-TotalPages` tells you how many pages). Match the user's described need against each
   term's `name` and `slug`.

   ```
   GET /wp/v2/consultant_categories?per_page=100&page=1&_fields=id,name,slug,count
   ```

   Prefer terms with a non-zero `count` — those have consultants attached. If nothing matches
   confidently, fall back to step 2's free-text search rather than guessing a term id.

2. **List consultants in that specialty.**
   Call `listConsultant`, filtering by the taxonomy and embedding the portrait in one round trip.

   ```
   GET /wp/v2/consultant?consultant_categories=<term_id>&per_page=10&_embed
   ```

   For free-text instead of taxonomy matching, use `search=<query>` on the same operation.
   Read `X-WP-Total` for the size of the result set before offering to page further.

3. **Read a single profile when the user picks one.**
   Call `getConsultant` with the numeric `id`.

   ```
   GET /wp/v2/consultant/{id}?_embed
   ```

   Useful fields: `title.rendered` (name), `content.rendered` (bio, HTML), `link` (the public
   profile page to hand the user), and `_embedded['wp:featuredmedia'][0].source_url` (portrait).

4. **Hand off to booking.**
   The API exposes no scheduling or availability. Direct the user to the consultant's `link`, or
   to `https://client.labayh.net/book-instant` for an instant session. **Never imply you have
   booked anything** — you cannot.

## Conventions

- **Pagination:** `page` + `per_page` (max 100). Totals arrive in `X-WP-Total` and
  `X-WP-TotalPages`; `Link` headers carry `rel="next"` / `rel="prev"`.
- **Sparse fields:** `_fields=id,slug,title,link` keeps taxonomy sweeps cheap.
- **Embedding:** `_embed` inlines featured media and terms — always prefer it over N+1 calls.

## Errors

Branch on the `code` slug, never the `message` (messages are returned in Arabic).

- `rest_invalid_param` (400) — usually `per_page` above 100. Clamp and page instead.
- `rest_post_invalid_id` (404) — the consultant id does not exist. Re-resolve from the listing.
- `rest_cannot_create` / `rest_forbidden` (401/403) — you attempted a write. This surface is
  read-only for you; do not retry with different credentials.

See `errors/labayh-problem-types.yml` for the full catalog.

## Safety

This is **health-adjacent** content. Return factual directory information — names, specialties,
languages, profile links. Do **not** diagnose, do not recommend a specific clinician as
medically appropriate, and do not interpret a user's symptoms into a specialty on your own
authority; present the taxonomy options and let the user choose. If a user expresses crisis or
self-harm intent, surface local emergency services and Labayh's support line (920031893)
immediately rather than continuing a directory search.
