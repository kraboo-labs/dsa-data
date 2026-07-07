# dsa-data — open mirror of the EU Trusted Flaggers register

[![Trusted flaggers](https://img.shields.io/endpoint?url=https%3A%2F%2Fapi.dsa-api.com%2Fv1%2Fbadge%2Fflaggers)](https://api.dsa-api.com/v1/trusted-flaggers)
[![Last sync](https://img.shields.io/endpoint?url=https%3A%2F%2Fapi.dsa-api.com%2Fv1%2Fbadge%2Ffreshness)](https://status.dsa-api.com)
[![Data license: CC BY 4.0](https://img.shields.io/badge/data-CC%20BY%204.0-blue.svg)](LICENSE)
[![API](https://img.shields.io/badge/API-dsa--api.com-3346ea.svg)](https://dsa-api.com)

Machine-readable copy of the list of designated Trusted Flaggers under the
EU Digital Services Act (Article 22(5)). Refreshed automatically by the
scraper at https://github.com/kraboo-labs/dsa-api whenever the upstream
list changes.

> Prefer to query by country, area, or domain, or get a change stream? Use the
> REST API: **https://dsa-api.com** · citing this dataset? See [`CITATION.cff`](CITATION.cff).

## Authoritative source

The European Commission publishes the official list at:

https://digital-strategy.ec.europa.eu/en/policies/trusted-flaggers-under-dsa

That page is the source of truth. This repository is a convenience
mirror. If you need to act on legal or compliance decisions, verify
against the official page.

## What's in `data/`

| File | Format | What it is |
|------|--------|------------|
| `trusted-flaggers.json` | JSON, pretty-printed, ordered by name | Current state of every TF, including normalized fields (country code, areas of expertise enum, etc.) |
| `trusted-flaggers.csv` | CSV, header row | Flat view with the most useful columns, suitable for spreadsheets and data tools |
| `changelog.json` | JSON | Append-only event log: every created / updated / removed / restored event since this mirror started |
| `source-snapshots/YYYY-MM-DD.html` | HTML | Frozen copy of the upstream page at the time of each scrape (added when changes were detected) |

## Schema

See the full normalized schema and the area-of-expertise enum in the
main repo PRD:
https://github.com/kraboo-labs/dsa-api/blob/main/prds/active/PRD_dsa-flaggers-api.md

In short, each TF row has:
- `id` — stable UUID derived from (name, DSC name, designation date)
- `name`, `email`, `email_domain`, `website`, `address_raw`
- `country_code`, `dsc_name`, `dsc_country_code`
- `areas_of_expertise_raw` — labels as published by the EU
- `areas_of_expertise` — normalized to a fixed enum; unknown labels map
  to `other` and the raw string is preserved alongside
- `designation_date`, `status`
- `first_seen_at`, `last_seen_at` — when our scraper first/last saw the entry

## Update cadence

The scraper runs every 6 hours. New commits land here only when the
upstream list actually changed (additions, edits, removals,
re-instatements). Idle days produce no commits.

Each commit message summarizes the change:
`data: 2 created, 1 updated, 0 removed, 0 restored (timestamp)`

## How to use this without the API

Just fetch the raw file:

```
curl -sSL https://raw.githubusercontent.com/kraboo-labs/dsa-data/main/data/trusted-flaggers.json
```

or pin a specific commit if you want a reproducible point-in-time view.

If you'd rather query by country, area, email domain, or get
change-stream and lookup endpoints, use the REST API instead:
https://dsa-api.com (docs at https://dsa-api.com/docs).

## License

- Data (everything under `data/`): CC BY 4.0 — see `LICENSE`.
- Any code in this repo: MIT — see `LICENSE-CODE`.

When you build on this data, please credit
"dsa-api.com — open mirror of the EU Trusted Flaggers register"
with a link back to this repository.

## Disclaimer

This is a community mirror. It is not the authoritative source, and we
make no compliance guarantees. The Commission's page is. See above.
