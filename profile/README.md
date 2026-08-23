<p align="center">
  <img src="https://raw.githubusercontent.com/mimir-media-db/mmdb-meta/master/assets/logo.png" width="180" alt="MMDB Logo" />
</p>

<h1 align="center">Mimir Media Database</h1>

<p align="center">
  <em>A free, open-source media metadata database — distributed as plain JSON via Git.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/dynamic/json?url=https://raw.githubusercontent.com/mimir-media-db/mmdb-meta/master/ingestion/state.json&query=$.totals.movies&label=Movies&color=2563eb&style=flat-square" alt="Movies" />
  <img src="https://img.shields.io/badge/dynamic/json?url=https://raw.githubusercontent.com/mimir-media-db/mmdb-meta/master/ingestion/state.json&query=$.totals.series&label=Series&color=16a34a&style=flat-square" alt="Series" />
  <img src="https://img.shields.io/badge/dynamic/json?url=https://raw.githubusercontent.com/mimir-media-db/mmdb-meta/master/ingestion/state.json&query=$.totals.people&label=People&color=ea580c&style=flat-square" alt="People" />
  <img src="https://img.shields.io/badge/license-MIT-gray?style=flat-square" alt="MIT License" />
</p>

---

## 📊 Database Stats

| Metric | Value |
|--------|-------|
| 🎬 Movies | 125,860 |
| 📺 Series | 31,728 |
| 👤 People | 172,962 |
| 📅 Year Range | 2000–2026 |
| 📁 Year Repos | 27 |
| 👥 People Repos | 26 (A–Z) |

## 🏆 Top 5 Years by Movie Count

| Repository | Movies | Series |
|-----------|--------|--------|
| [mmdb-2016](https://github.com/mimir-media-db/mmdb-2016) | 8,324 movies | 1,232 series |
| [mmdb-2017](https://github.com/mimir-media-db/mmdb-2017) | 6,306 movies | 1,184 series |
| [mmdb-2015](https://github.com/mimir-media-db/mmdb-2015) | 6,241 movies | 1,432 series |
| [mmdb-2018](https://github.com/mimir-media-db/mmdb-2018) | 6,201 movies | 1,236 series |
| [mmdb-2014](https://github.com/mimir-media-db/mmdb-2014) | 6,008 movies | 1,457 series |

---

## What is MMDB?

MMDB is a structured media metadata database stored entirely as **plain JSON files in public Git repositories**. No API keys, no rate limits, no vendor lock-in. Clone what you need and query locally.

- **Movies, series, seasons, episodes, and people** — versioned JSON schemas
- **Automated ingestion** from [Wikidata](https://www.wikidata.org/) — 6× daily
- **Sharded by year** — one repository per release year (2000–2026)
- **People sharded alphabetically** — one repository per letter (A–Z)
- **MIT licensed** — free for any use, commercial or personal

## 🌟 Vision

The long-term goal: a **comprehensive, community-driven alternative** to proprietary media APIs (TMDB, OMDB, etc.). Evergreen data anyone can fork, mirror, and build upon — without accounts, tokens, or usage caps.

## 🚀 How to Use

```bash
# Clone a year repo (shallow for speed)
git clone --depth 1 https://github.com/mimir-media-db/mmdb-2010

# Browse movies
ls mmdb-2010/data/movies/

# Query with jq
cat mmdb-2010/data/movies/index.json | jq '.[0:3]'

# Find a specific title
cat mmdb-2010/data/movies/index.json | jq '.[] | select(.title | test("Inception"; "i"))'

# People database (sharded by first letter of last name)
git clone --depth 1 https://github.com/mimir-media-db/mmdb-people-c
cat mmdb-people-c/data/people/index.json | jq 'length'
```

## 📦 Repository Map

| Repository | Purpose | Status |
|-----------|---------|--------|
| [mmdb-schema-and-tools](https://github.com/mimir-media-db/mmdb-schema-and-tools) | JSON schemas, validation, tooling, ingestion pipeline | ✅ Active |
| [mmdb-meta](https://github.com/mimir-media-db/mmdb-meta) | Cross-repo metadata registry & stats | ✅ Active |
| [mmdb-people-a](https://github.com/mimir-media-db/mmdb-people-a)…[mmdb-people-z](https://github.com/mimir-media-db/mmdb-people-z) | People database sharded A–Z (26 repos) | ✅ Active |
| [mmdb-2000](https://github.com/mimir-media-db/mmdb-2000)…[mmdb-2026](https://github.com/mimir-media-db/mmdb-2026) | Year-sharded movie & series data (27 repos) | ✅ Active |

## 📐 Schema Overview

Each entity follows a versioned JSON Schema (`v1`):

| Entity | Description | ID Format | Example |
|--------|-------------|-----------|---------|
| **Movie** | Films, documentaries, shorts | `m_<slug>_<year>` | `m_inception_2010` |
| **Series** | TV shows, streaming series | `s_<slug>` | `s_breaking_bad` |
| **Season** | Subdivision of a series | `s_<slug>_season_<nn>` | `s_breaking_bad_season_01` |
| **Episode** | Single episode | `e_<slug>_s<nn>e<nn>` | `e_breaking_bad_s01e01` |
| **Person** | Actors, directors, writers | `p_<slug>` | `p_christopher_nolan` |

Full schemas: [mmdb-schema-and-tools/schema/](https://github.com/mimir-media-db/mmdb-schema-and-tools/tree/master/schema)

## ⚙️ Ingestion

```
Wikidata (SPARQL) → Normalize → PR to data repo → Auto-merge
```

- **All years populated** — 2000–2026 fully ingested
- **6× daily** — Continuous updates for new and modified entries
- **Nightly** — Current year titles refreshed
- **Bot identity** — All automated commits via `mmdb-bot[bot]`
- **Safeguards** — Concurrency locks, anomaly detection, kill switch

## 🤝 Contributing

We welcome contributions! See the [Contribution Guide](https://github.com/mimir-media-db/mmdb-schema-and-tools/blob/master/docs/contribution-guide.md) for:

- Reporting data issues
- Submitting schema improvements
- Adding new entity types
- Improving tooling

## 📄 License

All MMDB repositories are licensed under the [MIT License](https://opensource.org/licenses/MIT).

---

<p align="center">
  <sub>Last auto-updated: 2026-08-23</sub>
</p>
