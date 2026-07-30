# Labayh

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
