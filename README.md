# Conference Deadlines

A static website tracking upcoming submission deadlines for software engineering, programming languages, and security conferences.

## Running Locally

Browsers block `fetch()` on local files. Serve with any static HTTP server:

```bash
python3 -m http.server 8080
```

Then open [http://localhost:8080](http://localhost:8080).

## Files

| File | Purpose |
|------|---------|
| `index.html` | Static website |
| `conferences.json` | Conference metadata (name, field, CORE rank, sponsor) |
| `deadlines.json` | Submission deadlines per conference/year/track |

## Conferences Included

**Software Engineering:** ICSE, FSE, ASE, ICSME, SANER, MSR, APSEC

**Programming Languages:** PLDI, POPL, OOPSLA, ECOOP, ICFP

**Security:** USENIX Security, CCS, ESORICS, TrustCom, S&P, ASIACCS, Euro S&P

## Website Features

- **Map tab** (default): World map with conference venue pins. Green = upcoming deadline, amber = deadline within 30 days, grey = deadline passed. Click a pin for details.
- **Deadlines tab**: Filterable/sortable table of deadlines.
  - Filter by field, CORE rank, deadline range, location, and sponsor
  - Sort by clicking column headers

## Updating `deadlines.json`

Add new entries following the schema:

```json
{
  "name": "Full conference name for the year",
  "acronym": "ICSE",
  "acronymx": "ICSE'27",
  "url": "https://...",
  "track": "research",
  "submission-deadline": "2026-06-30",
  "notification": "2026-10-10",
  "cfp": "https://...",
  "venue": {
    "city": "Dublin",
    "address": "Convention Centre Dublin, Dublin, Ireland",
    "lat": 53.3498,
    "lon": -6.2603,
    "region": "Europe"
  },
  "dates": "April 25 - May 1, 2027"
}
```

Valid `region` values: `Europe`, `Asia`, `Middle East`, `Australasia`, `North America`, `other`.

Remove entries for conferences that have already taken place.
