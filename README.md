# Labayh

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Labayh (لبيه) is a Saudi Arabian digital mental-health platform delivering online psychological,
psychiatric and family counselling to the Arabic-speaking Gulf market — scheduled and instant
sessions with 1,000+ licensed specialists, structured therapeutic programs, peer support groups,
assessments, mood tracking and webinars. It also runs Labayh Business (corporate wellbeing) and
Labayh Academy. Backed by 500 Global.

- Website: https://labayh.net
- Status: https://status.labayh.net/
- Business: https://business.labayh.net/

## API surface

Labayh operates **no developer program** — no developer portal, no published OpenAPI, no SDKs, no
CLI, no MCP server, no sandbox, no changelog, no `/.well-known/` documents and no security.txt.

What it does operate is a public, anonymously readable WordPress REST API at
`https://labayh.net/wp-json`. API Evangelist derived an OpenAPI 3.1 description of it from the
server's own discovery index and per-route OPTIONS schemas — **45 paths, 102 operations, 25
schemas**. Verified live at capture time:

| Resource | Count |
|---|---|
| Consultants | 875 |
| Consultant specialty categories | 255 |
| Blog articles | 2,162 |
| Media items | 3,565 |
| Recorded webinars | 33 |
| Support groups | 12 |
| Therapeutic programs | 10 |

Read operations need no credentials. Writes are advertised but require a WordPress application
password, and there is no public developer signup — treat the surface as read-only.

## Content licensing

`labayh.net/robots.txt` carries a Content-Signal block: `search=yes, ai-train=no, use=reference`,
disallowing GPTBot, Google-Extended, Applebot-Extended, Amazonbot, Bytespider and
meta-externalagent, and citing Article 4 of EU Directive 2019/790. Cite this content; do not train
on it. Captured verbatim as `well-known/labayh-robots.txt`.

## Artifacts

| Directory | What is in it |
|---|---|
| `openapi/` | Derived OpenAPI 3.1 description of the content API |
| `json-schema/` | Item schemas for consultant, program, support group, webinar, taxonomy — captured verbatim via OPTIONS |
| `examples/` | Real captured responses |
| `overlays/` | API Evangelist provenance and advisory annotations |
| `conventions/` | Pagination, sparse fields, `_embed`, context, filtering, bilingual behaviour |
| `errors/` | The WordPress error envelope and observed error codes (not RFC 9457) |
| `data-model/` | 19 entities, 35 relationships |
| `authentication/` | Application passwords + `X-WP-Nonce` |
| `lifecycle/` | Versioning, status page, and the absence of deprecation/SLA/changelog |
| `conformance/` | Standards the API does and does not meet |
| `well-known/` | The 404 sweep plus the Content-Signal robots.txt |
| `security/` | Domain security probe (TLS 1.3; no HSTS, DNSSEC or CAA; DMARC `p=none`) |
| `agentic-access/` | Recommended `x-agentic-access` contracts for all 102 operations |
| `mcp/` | Candidate 44-tool read-only MCP shape — Labayh publishes no MCP server |
| `skills/` | Three packaged Agent Skills over the read surface |
| `llms/` | llms.txt for the repo |

## Note on safety

This is a mental-health provider. Anything built on these artifacts should return directory and
catalogue facts rather than clinical advice, and should escalate any disclosure of crisis or
self-harm to local emergency services and Labayh's support line (920031893).
