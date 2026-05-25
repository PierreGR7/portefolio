---
title: Motorsport Data Pipeline (ETL + Dashboard)
description: Web scraping, SQLite, and Streamlit for multi-championship motorsport data
authors:
  - name: Pierre Graef
    to: ""
    avatar:
      src: https://i.pravatar.cc/128?u=2
image:
  src: /img/article_motorsport/cover.png
date: 2026-03-26T00:00:00.000Z
---

An end-to-end data engineering project collecting structured data on **motorsport championships, teams, circuits, and events** — from raw HTML scraping to a normalized SQLite database and an interactive Streamlit dashboard.

The project is structured around real data engineering principles: modular scrapers, a cleaning pipeline, schema-driven storage, and a visualization layer on top.

## Goals

- Automate the collection of motorsport data from public web sources (primarily Wikipedia).
- Build a clean, normalized relational database from heterogeneous HTML tables.
- Deliver an interactive dashboard for exploring championships, circuits, and teams.
- Practice good scraping hygiene: respecting `robots.txt`, adding request delays, using identifiable `User-Agent` headers.

## Architecture

```
scrapers/     → HTTP extraction (Wikipedia wikitables)
pipelines/    → Cleaning, normalization, orchestration
database/     → SQL schema + SQLite file (motorsport.db)
data/raw/     → Unversioned raw CSVs from scrapers
data/processed/ → Normalized CSVs ready for DB load
notebooks/    → Exploratory analysis
dashboard/    → Streamlit app
```

## ETL Pipeline

The pipeline is orchestrated by a single entry point (`build_dataset.py`) that accepts CLI flags for partial runs:

```bash
# Full pipeline
python pipelines/build_dataset.py

# Only Wikipedia championship data
python pipelines/build_dataset.py --only wiki

# Skip teams (faster iteration)
python pipelines/build_dataset.py --skip-teams
```

Each scraper targets a specific Wikipedia article structure (e.g., "List of … series" pages exposing `wikitable` HTML). The cleaning layer normalizes column names, deduplicates rows, fixes encoding issues, and outputs consistent CSVs.

## Database Schema

The SQLite database covers four main entities:

- `championships` — name, type, founding year, governing body
- `circuits` — name, location, country, length
- `teams` — constructor, nationality, active seasons
- `events` — calendar placeholder (extended in future versions)

All tables are replaced on each pipeline run to ensure reproducibility.

## Streamlit Dashboard

The dashboard provides an interactive interface to explore the loaded data:

- Championship browser with filter by type (circuit racing, rallying, karting…)
- Circuit map view by country
- Team directory with season activity ranges

## Tools & Technologies

- **Scraping**: Python (`requests`, `BeautifulSoup`)
- **Cleaning**: `pandas`
- **Storage**: SQLite (`sqlite3`)
- **Dashboard**: Streamlit
- **CI/CD**: GitHub Actions (linting + pipeline smoke test)
- **Version control**: Git + GitHub

## Project Links

- GitHub repository: https://github.com/PierreGR7/motorsport-championships-scrapping
