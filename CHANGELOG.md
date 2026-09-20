# Changelog

All notable changes to the EternalEngine public OpenAPI spec are documented in this file.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
Versioning: this repository's tags track the spec's own `info.version`.

## [1.1.0] - 2026-09-20

### Changed
- Synced from the monorepo's regenerated spec (`info.version` 1.0.0 → 1.1.0, bumped
  2026-09-19 in the source repository). The gateway precondition that gated live use of this
  API (GW-INV-08) was confirmed resolved, so the generator stopped emitting an
  `info.x-known-issue` field — this repo's spec never carried that field (it was published
  after the fix shipped), so the sync is a version/timestamp bump only. No operation, schema,
  or path changed: verified byte-identical after normalizing `info.version` and
  `info.x-generated-at`. Re-linted clean at the same 62 known warnings (23 `struct`, 1
  `info-license-strict`, 38 `operation-4xx-response` — unchanged counts, see README.md).

## [1.0.0] - 2026-09-19

### Added
- Initial publication of the public OpenAPI spec: `openapi.public.json` and
  `openapi.public.yaml`, 43 `GET` operations across PostFrame and PayGate, generated from
  `services/ee-postframe/openapi.yaml` and `services/ee-paygate/openapi.yaml`.
- `redocly.yaml` lint config and `.github/workflows/validate.yml`, running
  `@redocly/cli lint` on both files and a JSON/YAML consistency check on every push and PR.
- See README.md "Known lint findings" for the pre-existing, intentionally-unfixed lint findings
  in this initial publication.
