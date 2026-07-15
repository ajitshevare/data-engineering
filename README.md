# Learning Databricks 🚀

A hands-on project for learning Databricks fundamentals using a sample movie dataset. This repo documents my journey through data ingestion, transformation, and analysis on the Databricks Lakehouse Platform.

## 📋 Project Overview

This project uses a sample movie dataset (`movies_sample.csv`) to practice core Databricks skills, including:

- Uploading and ingesting CSV data
- Creating and managing tables with Unity Catalog
- Querying data with Spark SQL and PySpark
- Performing transformations and aggregations
- Working with Delta Lake tables
- Building simple visualizations and dashboards

## 📁 Dataset

**File:** `data/movies_sample.csv`

A sample dataset of 30 movies with the following schema:

| Column | Type | Description |
|---|---|---|
| `movie_id` | int | Unique identifier for each movie |
| `title` | string | Movie title |
| `genre` | string | Primary genre (Action, Drama, Sci-Fi, etc.) |
| `release_year` | int | Year of theatrical release |
| `director` | string | Director's name |
| `duration_min` | int | Runtime in minutes |
| `rating` | double | Average viewer rating (0–10 scale) |
| `budget_usd` | long | Production budget in US dollars |
| `box_office_usd` | long | Worldwide box office gross in US dollars |
| `language` | string | Primary language |
| `country` | string | Country of production |

## 🛠️ Getting Started

### Prerequisites

- A Databricks workspace (Free Edition, Community Edition, or a paid workspace)
- Basic familiarity with SQL and/or Python

### Step 1: Upload the Data

1. In your Databricks workspace, go to **Catalog → Add data → Upload file**
2. Select `movies_sample.csv`
3. Choose a catalog and schema, then create the table (e.g., `main.default.movies`)

Alternatively, upload to a volume and read it in a notebook:

```python
df = spark.read.csv(
    "/Volumes/main/default/my_volume/movies_sample.csv",
    header=True,
    inferSchema=True
)
df.display()
```

### Step 2: Create a Delta Table

```python
df.write.format("delta").mode("overwrite").saveAsTable("main.default.movies")
```

### Step 3: Explore with SQL

```sql
-- Average rating and total box office by genre
SELECT
  genre,
  ROUND(AVG(rating), 2) AS avg_rating,
  SUM(box_office_usd) AS total_box_office
FROM main.default.movies
GROUP BY genre
ORDER BY total_box_office DESC;
```

## 📚 Learning Exercises

Ideas to practice with this dataset:

1. **Filtering & sorting** — Find all movies released after 2020 with a rating above 7.5
2. **Aggregations** — Calculate average budget and box office per genre and per country
3. **Derived columns** — Add a `profit_usd` column (`box_office_usd - budget_usd`) and a `roi` column
4. **Window functions** — Rank movies by rating within each genre
5. **Joins** — Create a second table (e.g., directors or actors) and practice inner/left joins
6. **Delta Lake features** — Try `UPDATE`, `DELETE`, `MERGE`, and time travel (`DESCRIBE HISTORY`)
7. **Visualizations** — Build a bar chart of box office by genre in a notebook or dashboard

## 📂 Suggested Project Structure

```
learning-databricks/
├── README.md
├── data/
│   └── movies_sample.csv
├── notebooks/
│   ├── 01_data_ingestion.py
│   ├── 02_exploration_sql.sql
│   ├── 03_transformations.py
│   └── 04_delta_lake_basics.py
└── docs/
    └── notes.md
```

## 🔗 Useful Resources

- [Databricks Documentation](https://docs.databricks.com/)
- [Delta Lake Documentation](https://docs.delta.io/)
- [Apache Spark SQL Reference](https://spark.apache.org/docs/latest/sql-ref.html)
- [Databricks Free Edition](https://www.databricks.com/learn/free-edition)

## 📝 License

This project is for personal learning purposes. The dataset is synthetic sample data.
