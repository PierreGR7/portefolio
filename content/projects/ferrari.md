---
title: Scuderia Ferrari F1 - ETL to Dashboard (Python)
description: API, Sqlite and Streamlit
authors:
  - name: Pierre Graef
    to: ""
    avatar:
      src: https://i.pravatar.cc/128?u=2
image:
  src: /img/article_ferrari/ferrari-db-logo.png
date: 2025-05-25T01:00:00.000Z
---

As a lifelong **Ferrari** and **Formula 1** enthusiast, this project is more than just a technical case study—it’s a tribute to a childhood passion.

In 2023, I had the chance to visit **Maranello**, the heart of the Prancing Horse. Walking through the Ferrari Museum, seeing the legendary F1 cars up close, and breathing in decades of racing history lit a spark.

This project explores the **historical performance of Scuderia Ferrari** in Formula 1 from **1950 to today**, using real-world data to deliver insightful visualizations and interactive reporting tools.

Through this case study, I aimed to:

- Understand long-term performance trends of Ferrari in F1.
- Automate data ingestion, transformation, and reporting.
- Build a reproducible and maintainable data pipeline.
- Practice full-stack data engineering with **SQLite**, **Python**, and **Streamlit**.

![article\_chatbot.png](/img/article_ferrari/ferrari-db-logo.png)

### Tools & Technologies Used

- **Data source**: Ergast API (historical F1 race data)
- **Database**: SQLite
- **Data processing**: Python (requests, pandas, sqlite3)
- **Visualization / Dashboard**: Streamlit, Matplotlib, Plotly
- **Version control**: Git + GitHub

## Database Architecture

I designed a normalized relational database in SQLite to structure the racing data, using the following main tables:

- `drivers`: driver information
- `constructors`: constructor information
- `races`: Grand Prix calendar
- `circuits`: circuit names and locations
- `results`: race results (position, driver, constructor, points, status)
- *(Planned extensions:*&#x20;`lap_times`*,*&#x20;`pit_stops`*,*&#x20;`qualifying`*)*

Example of a SQL query :

```sql
SELECT year, COUNT(*) AS wins
FROM results
JOIN races ON results.raceId = races.raceId
WHERE constructorId = (SELECT constructorId FROM constructors WHERE name = 'Ferrari') 
  AND position = 1
GROUP BY year;
```

## ETL Pipeline

The data pipeline is fully automated via Python scripts that:

- Fetch season-by-season race results from the Ergast API
- Filter only Ferrari-related data
- Clean and normalize the datasets
- Store the records in SQLite tables

The process is **reusable and easily updatable** as new seasons become available.

## Interactive Streamlit Dashboard

Using **Streamlit**, I created a dashboard with multiple pages to explore Ferrari’s performance over time. Key components include:

- **Wins per season** (bar chart)
- **Points, podiums, pole positions per year** (line charts)
- **Top 10 Ferrari drivers** based on wins and total points
- **Ferrari’s most successful circuits** (interactive map)
- **Era-based analysis** (e.g., Schumacher dominance, post-2010 struggles)

## Key Insights

- **Best season**: 2004 (15 wins with Michael Schumacher)
- **Longest winless streak**: 1991–1993
- **Top driver**: Michael Schumacher (72 wins with Ferrari)
- **Most successful tracks**: Monza (Italy), Spa (Belgium), Silverstone (UK)

![dashboard-ferrari.png](/img/article_ferrari/dashboard-ferrari.png)

## Project Links

- GitHub repository: <https://github.com/PierreGR7/ferrari_f1_db.git>
- Live dashboard: <https://pierre-graef-scuderia-ferrari-dashboard.streamlit.app/>
