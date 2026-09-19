# Changelog

All notable changes to the EternalEngine public OpenAPI spec are documented in this file.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
Versioning: this repository's tags track the spec's own `info.version`.

## [1.0.0] - 2026-09-19

### Added
- Initial publication of the public OpenAPI spec: `openapi.public.json` and
  `openapi.public.yaml`, 43 `GET` operations across PostFrame and PayGate, generated from
  `services/ee-postframe/openapi.yaml` and `services/ee-paygate/openapi.yaml`.
- `redocly.yaml` lint config and `.github/workflows/validate.yml`, running
  `@redocly/cli lint` on both files and a JSON/YAML consistency check on every push and PR.
- See README.md "Known lint findings" for the pre-existing, intentionally-unfixed lint findings
  in this initial publication.
