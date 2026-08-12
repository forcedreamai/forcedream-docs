# Japan Distribution Tracker

Working record of Japanese developer-discovery surfaces.

**Rules.** No reach, traffic, or audience figures unless taken from a source that publishes them, with the source recorded. `Status` reflects what has happened, not what is planned. `Result` stays empty until a response is received. Every status change gets a date. Every requirement is verified from the site's own stated policy, with the URL recorded — never assumed.

**Status values:** `未提出` not yet submitted · `提出済み` submitted, awaiting response · `掲載中` listed and live · `却下` rejected · `保留` blocked

---

## The strategic finding

Japan distribution is mostly **earned, not submitted**.

Research into Japanese MCP discovery surfaces found that the visible "MCPサーバー一覧" pages — WEEL, スマートスタイル, ビジョナリージャパン, バイテック and similar — are editorially curated corporate blog articles with no submission process. There is nothing to apply to. Inclusion follows from being worth writing about.

The surfaces that do accept our own content are developer publishing platforms, and both major ones prohibit promotional posting. So the route into Japan is genuinely useful technical writing, not directory submission. Directory listings follow adoption; they do not create it.

---

## Directories — submission-based

| Name | URL | Type | Japanese requirement | Verified from | Submission | Account | Cost | Status | Date | Result |
|---|---|---|---|---|---|---|---|---|---|---|
| AI エージェントナビ | aiagent-navi.com | MCP directory | **Required — stated editorial policy** | Direct email from 運営事務局, 2026-08 | Email | — | Unknown | 却下 | 2026-08 | Rejected: official documentation available in English only. Reversal asset now exists — see below. |
| MCP Spaces (エムスペ) | mcp-spaces.com | MCP directory | Assumed required — **not yet verified** | — | **Not yet identified** | Unknown | Unknown | 未提出 | — | — |

**AI エージェントナビ reversal asset:** `github.com/forcedreamai/forcedream-mcp/blob/main/README.ja.md` — full Japanese documentation for the exact repository they reviewed, published 2026-08. Their stated requirement is now satisfied. Not yet re-submitted; gated on native review and branch merges.

**MCP Spaces** is the only genuine submission-based Japanese MCP directory found so far. Its submission process, requirements and cost are all unverified. Do not submit until its own stated policy has been read.

---

## Publishing platforms — content, not submission

These are not directories. There is no listing to apply for. The entry ticket is an article that stands on its own technical merit.

| Name | URL | Promotional posts | Verified from | Notes |
|---|---|---|---|---|
| Qiita | qiita.com | **Prohibited** as primary purpose — but technical explanation of one's own product explicitly does **not** count as promotion | help.qiita.com community guideline; qiita.com/terms | Has an Organization feature for company accounts, subject to the same guidelines. A real technical article is permitted; a product announcement is not. |
| Zenn | zenn.dev | **Prohibited** — June 2025 revision made advertising- or recruitment-primary articles an explicit violation | info.zenn.dev 2025-06-05; zenn.dev/guideline | Generative AI permitted for drafting, but posting without verifying accuracy is explicitly prohibited. Any drafted article must be human-verified before posting. Publication feature has separate corporate terms. |
| はてなブックマーク | b.hatena.ne.jp | Aggregator — not a posting target | — | Surfaces content published elsewhere. Not a distribution action in itself. |

**Consequence for content:** the first articles must be independently useful and only secondarily introduce ForceDream. A product-shaped article would be permitted on neither platform.

---

## Live listings — English, not Japan-specific

Recorded because Japanese developers do reach them, not because they satisfy a Japanese-language requirement.

| Name | URL | Status | Notes |
|---|---|---|---|
| Smithery | smithery.ai/servers/forcedreamai/mcp-server | 掲載中 | Acquired by Arcade.dev. Tool count needs updating to 17. |
| Glama | glama.ai | 掲載中 | |
| Official MCP Registry | registry.modelcontextprotocol.io | 掲載中 | `io.github.forcedreamai/mcp-server` |
| GitHub | github.com/forcedreamai/forcedream-mcp | 掲載中 | `README.ja.md` published 2026-08 |

---

## Communities — not yet assessed

Each needs its own stated policy read before any action. All currently 未提出 with requirements unverified.

| Name | Type | Notes |
|---|---|---|
| connpass | Developer events | Event listing platform. Requires an actual event, not a product listing. |
| Findy / Forkwell | Engineer communities | Primarily recruitment-oriented. Fit unverified. |
| Google Cloud Japan community groups | Cloud community | Eligibility not assessed. Do not assume. |

---

## Blocked

| Item | Blocked on | Owner |
|---|---|---|
| AI エージェントナビ re-submission | Native Japanese review; branch merges | Engineer 3 / Damien |
| Article 1 (MCP vs A2A) | One **successful** A2A execution to describe. The only run to date dead-lettered on `insufficient_funds`. | Engineer 3, needs funded key |
| Japanese `/mcp` landing page | No i18n pipeline on `.ai`; CJK font absent from the stack | Engineer 1 |
| Japanese search indexability | No `/ja` route exists on either domain | Engineer 1 |
| Locale attribution | Nothing to attribute from until Japanese pages exist | Engineer 3, after Engineer 1 |
| Getting Started / SDK / Workflows / AI Playbooks (JA) | English documents describe SDK methods that do not exist | Engineer 2 |
| Billing / Pricing / Economics / Agent Selection (JA) | Percentages unconfirmed; margin-vs-gross open | Engineer 2 |

---

## Not a distribution surface

- **Google for Startups Japan accelerator** — targets Japanese growth-stage startups. Not a developer-discovery channel.
- **Corporate MCP listicles** (WEEL, スマートスタイル, ビジョナリージャパン, バイテック) — editorial, no submission process. Cannot be applied to.
