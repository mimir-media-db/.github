<p align="center">
  <img src="https://raw.githubusercontent.com/mimir-media-db/mmdb-meta/main/assets/logo.png" width="180" alt="MMDB Logo" />
</p>

<h1 align="center">Mimir Media Database</h1>

<p align="center">
  <em>A free, open-source media metadata database — distributed as plain JSON via Git.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/dynamic/json?url=https://raw.githubusercontent.com/mimir-media-db/mmdb-meta/main/stats.json&query=$.movies&label=Movies&color=2563eb&style=flat-square" alt="Movies" />
  <img src="https://img.shields.io/badge/dynamic/json?url=https://raw.githubusercontent.com/mimir-media-db/mmdb-meta/main/stats.json&query=$.series&label=Series&color=16a34a&style=flat-square" alt="Series" />
  <img src="https://img.shields.io/badge/dynamic/json?url=https://raw.githubusercontent.com/mimir-media-db/mmdb-meta/main/stats.json&query=$.people&label=People&color=ea580c&style=flat-square" alt="People" />
  <img src="https://img.shields.io/badge/dynamic/json?url=https://raw.githubusercontent.com/mimir-media-db/mmdb-meta/main/stats.json&query=$.year_repos.count&label=Year%20Repos&color=7c3aed&style=flat-square" alt="Year Repos" />
  <img src="https://img.shields.io/badge/license-MIT-gray?style=flat-square" alt="MIT License" />
</p>

---

## What is MMDB?

MMDB is a structured media metadata database stored entirely as **plain JSON files in public Git repositories**. No API keys, no rate limits, no vendor lock-in. Clone what you need and query locally.

- **Movies, series, seasons, episodes, and people** — versioned JSON schemas
- **Automated ingestion** from [Wikidata](https://www.wikidata.org/) — 3× daily
- **Sharded by year** — one repository per release year (1888–present)
- **MIT licensed** — free for any use, commercial or personal

## Vision

The long-term goal: a **comprehensive, community-driven alternative** to proprietary media APIs (TMDB, OMDB, etc.). Evergreen data anyone can fork, mirror, and build upon — without accounts, tokens, or usage caps.

## Quick Start

```bash
# Clone a year repo (shallow for speed)
git clone --depth 1 https://github.com/mimir-media-db/mmdb-2010

# Browse movies
ls mmdb-2010/data/movies/

# Query with jq
cat mmdb-2010/data/movies/index.json | jq '.[0:3]'

# Find a specific title
cat mmdb-2010/data/movies/index.json | jq '.[] | select(.title | test("Inception"; "i"))'

# People database
git clone --depth 1 https://github.com/mimir-media-db/mmdb-people
cat mmdb-people/data/people/index.json | jq 'length'
```

## Repository Map

| Repository | Purpose | Status |
|-----------|---------|--------|
| [mmdb-schema-and-tools](https://github.com/mimir-media-db/mmdb-schema-and-tools) | JSON schemas, validation, tooling, ingestion pipeline | ✅ Active |
| [mmdb-meta](https://github.com/mimir-media-db/mmdb-meta) | Cross-repo metadata registry & stats | ✅ Active |
| [mmdb-people](https://github.com/mimir-media-db/mmdb-people) | Global people database (cast, crew) | ✅ Active |
| [mmdb-2009](https://github.com/mimir-media-db/mmdb-2009)…[mmdb-2026](https://github.com/mimir-media-db/mmdb-2026) | Year-sharded movie & series data | 🔄 Growing |

## Schema Overview

Each entity follows a versioned JSON Schema (`v1`):

| Entity | Description | ID Format | Example |
|--------|-------------|-----------|---------|
| **Movie** | Films, documentaries, shorts | `m_<slug>_<year>` | `m_inception_2010` |
| **Series** | TV shows, streaming series | `s_<slug>` | `s_breaking_bad` |
| **Season** | Subdivision of a series | `s_<slug>_season_<nn>` | `s_breaking_bad_season_01` |
| **Episode** | Single episode | `e_<slug>_s<nn>e<nn>` | `e_breaking_bad_s01e01` |
| **Person** | Actors, directors, writers | `p_<slug>` | `p_christopher_nolan` |

Full schemas: [mmdb-schema-and-tools/schema/](https://github.com/mimir-media-db/mmdb-schema-and-tools/tree/master/schema)

## How Ingestion Works

```
Wikidata (SPARQL) → Normalize → PR to year repo → Auto-merge
```

- **3× daily** — Bidirectional backlog (forward from 2010, backward from 2009)
- **Nightly** — Current year titles
- **Bot identity** — All automated commits via `mmdb-bot[bot]`
- **Safeguards** — Concurrency locks, anomaly detection, kill switch

## Contributing

We welcome contributions! See the [Contribution Guide](https://github.com/mimir-media-db/mmdb-schema-and-tools/blob/master/docs/contribution-guide.md) for:

- Reporting data issues
- Submitting schema improvements
- Adding new entity types
- Improving tooling

## License

All MMDB repositories are licensed under the [MIT License](https://opensource.org/licenses/MIT).
