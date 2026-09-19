# EternalEngine Public API — OpenAPI spec

The public, read-only OpenAPI 3.1 specification for the EternalEngine platform: **43 `GET`
operations**, covering every service whose data a tenant's own API key can authenticate
(currently PostFrame and PayGate). No write operation (`POST`/`PUT`/`PATCH`/`DELETE`) is
published here — this document, and everything generated from it, can only ever read your data.

- **`openapi.public.json`** and **`openapi.public.yaml`** — the same document in both formats
  (verified byte-consistent on every push by [`.github/workflows/validate.yml`](./.github/workflows/validate.yml)).
- Rendered human-readably at [eternalengineos.io/developers](https://eternalengineos.io/developers).
- Consumed directly by [EternalEngineOS/ee-mcp-public](https://github.com/EternalEngineOS/ee-mcp-public),
  a distributable MCP server generated from this spec at build time.

## Base URL

```
https://app.eternalengineos.io/api/v1
```

## Authentication

Every operation requires a Bearer API key from the service that owns it — **your own key**,
scoped to your tenant:

| App | Key prefix | Mint from |
|---|---|---|
| PostFrame (email, contacts, audiences, analytics) | `pf_live_*` / `pf_test_*` | `app.eternalengineos.io/postframe/developers` |
| PayGate (transactions, disputes, customers, payouts) | `pg_live_*` / `pg_test_*` | `app.eternalengineos.io/paygate/developers` |

Get a key from the [EternalEngine developers page](https://eternalengineos.io/developers) —
minting one requires signing in to `app.eternalengineos.io`.

One operation, `GET /paygate/status`, requires no authentication at all (`security: []` in the
spec) — a plain service-health check.

## Read-only

Only `GET` operations are published in this spec. Every write operation on the underlying
service is deliberately excluded at generation time — this is a structural property of the
generator, not a convention someone could accidentally violate by adding an operation here.

## Rate limits

Each authenticated operation carries an `x-rate-limit` extension (e.g. `100` requests per `1m`),
sourced from the EternalEngine gateway's own rate limiter — the gateway is the actual authority;
this field documents what it enforces. Some operations also sit behind a narrower service-level
limit (for example PayGate's transaction endpoints); where that applies it is noted in the
operation's `description`.

## Example request

```bash
curl -H "Authorization: Bearer pf_live_your_key" \
  "https://app.eternalengineos.io/api/v1/postframe/emails?limit=10"
```

No-auth example (service health):

```bash
curl "https://app.eternalengineos.io/api/v1/paygate/status"
```

## Get your API key

See [eternalengineos.io/developers](https://eternalengineos.io/developers) — "Get your API key".

## Using this spec

- **With an MCP client / AI agent:** run [EternalEngineOS/ee-mcp-public](https://github.com/EternalEngineOS/ee-mcp-public),
  which is generated directly from `openapi.public.json` — one tool per `GET` operation.
- **With any OpenAPI tool:** point it at `openapi.public.yaml` or `openapi.public.json` directly
  (e.g. `npx @redocly/cli preview-docs openapi.public.yaml`, an SDK generator, a Postman import).

## Versioning

This repository tracks `info.version` in the spec (currently **`1.0.0`**). A version bump and a
[`CHANGELOG.md`](./CHANGELOG.md) entry accompany any change to the published operations. Adding a
new `GET` operation is additive (patch or minor); removing or renaming one, or narrowing its
response shape, is a breaking (major) change and will be called out explicitly in the changelog.

## Known lint findings

CI runs [`@redocly/cli lint`](https://redocly.com/docs/cli/) against both files on every push
(see [`redocly.yaml`](./redocly.yaml)). As of `1.0.0`, the spec has pre-existing findings that
are **known and intentionally not fixed here** — fixing them means changing what the underlying
services' own `openapi.yaml` files say, which is out of this repository's scope (this repo
mirrors the generated output; it does not own the source specs). The corresponding rules are
downgraded to `warn` so CI reports them without blocking:

| Rule | Count | What it flags |
|---|---|---|
| `struct` | 23 | Several response schemas use OpenAPI 3.0-style `nullable: true` on a property, which is not valid under the declared `openapi: 3.1.0` (3.1 expects a `type` array including `null` instead). |
| `info-license-strict` | 1 | `info.license.name` is `"Proprietary"`, which is not a recognized SPDX identifier — expected, since the underlying API is not open source (this is a statement about the *API's* license, distinct from this repository's own [LICENSE](./LICENSE) for the spec document itself). |
| `operation-4xx-response` | 38 | Most operations document only their `200` response, not their `4xx` error shapes. |

Run `npx @redocly/cli lint openapi.public.yaml` locally to see the full list.

## License

The spec document in this repository (`openapi.public.json` / `openapi.public.yaml`) is licensed
[CC BY 4.0](./LICENSE) — you may share and adapt it, with attribution. This license covers the
specification document itself; it does not grant any right to use the EternalEngine API without
your own API key and account, and it does not change the underlying API's own terms of service.

## See also

- [EternalEngineOS/ee-mcp-public](https://github.com/EternalEngineOS/ee-mcp-public) — the MCP
  server generated from this spec.
- [eternalengineos.io/developers](https://eternalengineos.io/developers) — human-readable docs.
