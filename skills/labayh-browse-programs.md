---
name: Browse Labayh therapeutic programs and support groups
description: Enumerate Labayh's structured therapeutic programs, peer support groups and recorded webinars, with their categories, so a user can compare what the platform offers.
api: openapi/labayh-content-openapi.yml
operations:
  - listProgram
  - getProgram
  - listProgramCategories
  - listSupportGroup
  - getSupportGroup
  - listSupportGroupCategories
  - listRecordedWebinar
  - listLiveWebinar
generated: '2026-07-19'
method: generated
source: openapi/labayh-content-openapi.yml
---

# Browse Labayh therapeutic programs and support groups

Beyond one-to-one sessions, Labayh publishes three group-format offerings through the same
content API: **structured therapeutic programs** (10 at capture time), **peer support groups**
(12), and **recorded webinars** (33). A `live_webinar` type is registered but was empty when
this skill was written — always check the count rather than assuming.

**Base URL:** `https://labayh.net/wp-json`
**Auth:** none — all read operations are anonymous.

## Steps

1. **Enumerate therapeutic programs.**

   ```
   GET /wp/v2/program?per_page=100&_embed
   ```

   `listProgram` returns the full set in one page. Read `X-WP-Total` to confirm the count
   before summarising — the catalogue is small and changes without notice.

2. **Label them by category.**

   ```
   GET /wp/v2/program_categories?per_page=100&_fields=id,name,slug,count
   ```

   `listProgramCategories` gives the term names. Each program's `program_categories` property is
   an array of these term ids — join them client-side; there is no server-side expansion.

3. **Enumerate support groups the same way.**

   ```
   GET /wp/v2/support_group?per_page=100&_embed
   GET /wp/v2/support_group_categories?per_page=100&_fields=id,name,slug,count
   ```

   Use `listSupportGroup` and `listSupportGroupCategories`. For one group's detail call
   `getSupportGroup` with its `id`.

4. **Check webinars.**

   ```
   GET /wp/v2/recorded_webinar?per_page=100&_embed
   GET /wp/v2/live_webinar?per_page=100
   ```

   `listRecordedWebinar` is the on-demand library. `listLiveWebinar` is the scheduled surface —
   if `X-WP-Total` is `0`, say there are no live sessions listed rather than reporting an error.

5. **Present the comparison.**
   For each item read `title.rendered` for the name, `content.rendered` for the description
   (HTML — strip it), `link` for the public page, and
   `_embedded['wp:featuredmedia'][0].source_url` for artwork. Always include the `link` so the
   user can enrol themselves.

## Conventions

- `per_page` maxes at **100** — these catalogues fit in a single page, so one call each is enough.
- `_embed` inlines featured media; `_fields` trims taxonomy sweeps.
- Content is **Arabic-first**; translate for English-speaking users and say that you have.

## What this API does not give you

There is **no pricing, no schedule, no availability and no enrolment endpoint**. The content API
describes the offerings only. Do not state a price or a start date — none is published. Route the
user to the item's `link`, or to `https://client.labayh.net/` to enrol.

## Errors

Branch on `code`, not `message` (Arabic). `rest_invalid_param` (400) means `per_page` exceeded
100. `rest_no_route` (404) means the content type is no longer registered — re-read
`https://labayh.net/wp-json/` for the current route table, since types come and go with the
site's plugins. Full catalog in `errors/labayh-problem-types.yml`.

## Safety

Describe offerings factually. Do not recommend a program as clinically appropriate for a user's
described symptoms — that is a decision for a licensed specialist. If a user is in crisis,
surface emergency services and Labayh's line (920031893) before anything else.
