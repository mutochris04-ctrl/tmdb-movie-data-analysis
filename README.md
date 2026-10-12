# TMDB Movie Data Analysis

A movie data analysis pipeline built with Python and Pandas for the AmaliTech DEM02 lab
(Python Programming with Pandas and NumPy).

The project fetches data for a list of movies from the TMDB API, cleans and transforms it,
calculates KPIs (revenue, profit, ROI, ratings, popularity), compares franchises and directors,
and presents the results with charts.

## Project files

| File | What it contains |
|------|------------------|
| `tmdb_analysis.ipynb` | The complete workflow: API extraction, cleaning, KPIs, searches, franchise/director analysis and charts |
| `REPORT.md` | Final report: methodology, key insights and conclusions |
| `requirements.txt` | Python libraries used |
| `.gitignore` | Keeps the secret `.env` file and cache files out of GitHub |

## How to run

1. Install the libraries:

        pip install -r requirements.txt

2. Get a free API key from https://www.themoviedb.org/settings/api
3. Create a file called `.env` in the project folder with this line:

        TMDB_API_KEY=your_key_here

4. Open `tmdb_analysis.ipynb` in VS Code or Jupyter and click **Run All**.

The API key is read from `.env` with `python-dotenv`, so it is never written in the code or uploaded to GitHub.

## Workflow steps

1. **API extraction** – fetch each movie ID (with cast and crew) and store the results in a DataFrame. Invalid IDs (e.g. `0`) are skipped.
2. **Cleaning** – drop irrelevant columns, extract names from JSON-like columns, extract cast/director, fix data types, replace zeros and placeholders with NaN, convert money to million USD, remove duplicates, keep released movies, reorder columns.
3. **KPIs** – a ranking function (UDF) for the 10 best/worst rankings, two advanced searches, franchise vs standalone comparison, most successful franchises and directors.
4. **Visualization** – 5 Matplotlib charts.
