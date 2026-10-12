# Final Report – TMDB Movie Data Analysis

## 1. Goal

Build a movie data analysis pipeline in Python and Pandas: fetch movie data from the TMDB API,
clean it, calculate key performance indicators (KPIs), and present the findings with charts.

## 2. Methodology

**Data extraction.** The 19 movie IDs from the lab were requested one by one from the TMDB API
(`/movie/{id}` with `append_to_response=credits` to also get cast and crew). Each request was
checked with an `if/else` on the status code. ID `0` is not a real movie and was skipped,
so the dataset has **18 movies**. Each request has a 10-second timeout and a `try/except`, so a network
error prints a message instead of stopping the notebook. The API key is stored in a `.env` file and is not
in the code. **Data was extracted from the TMDB API on 12 October 2026**; TMDB values such as popularity,
vote counts and revenue change over time, so running the notebook later can give slightly different numbers.

**Cleaning and transformation.**
- Dropped 5 irrelevant columns (`adult`, `imdb_id`, `original_title`, `video`, `homepage`).
- Turned JSON-like columns into readable text with small functions, e.g. genres become `Action|Adventure`, and the collection becomes its name.
- Inspected the extracted columns with `value_counts()`. No empty or strange values were found; one anomaly is that the same genres can appear in a different order (e.g. `Adventure|Action|Science Fiction` and `Science Fiction|Action|Adventure`), so they are counted as different values. The genre chart splits genres one by one, so the analysis is not affected.
- Extracted the cast list, cast size, director and crew size from the credits.
- Converted columns to numbers and dates; values of `0` in budget, revenue and runtime were replaced with NaN because 0 means "unknown".
- Converted budget and revenue to **million USD**.
- Set ratings to NaN for movies with 0 votes, and replaced placeholder text (e.g. "No Data") with NaN.
- Removed duplicates, rows without id/title, rows with fewer than 10 values, and kept only released movies.
- Reordered to the 22 columns requested by the lab. All 18 movies passed every cleaning check.

**Analysis.** Profit (revenue − budget) and ROI (revenue ÷ budget) were added. A user-defined
function `rank_movies()` sorts any column from highest or lowest, and was reused for all 10 rankings.
ROI rankings only use movies with a budget of at least 10M, and rating rankings only movies with at
least 10 votes. Franchises and directors were analysed with `groupby` and `agg`.

## 3. Key insights

**Revenue and profit**
- *Avengers: Endgame* has the highest revenue (**$2,927M**), just ahead of *Avatar* ($2,924M).
- *Avatar* has the highest **profit** ($2,687M), because its budget ($237M) was lower than Endgame's ($356M).
- Every movie in the dataset made a profit of more than $1,000M; the "lowest" profit is *Star Wars: The Last Jedi* ($1,034M).

**Return on investment**
- Best ROI: *Avatar* (12.3×), *Titanic* (11.3×) and *Jurassic World* (11.1×).
- Lowest ROI: *Star Wars: The Last Jedi* (4.4×), the movie with one of the biggest budgets ($300M).
- A bigger budget does not guarantee a better return.

**Ratings and popularity**
- Highest rated: *Avengers: Infinity War* (8.25) and *Avengers: Endgame* (8.20).
- Lowest rated: *Jurassic World: Fallen Kingdom* (6.5).
- Most voted: *The Avengers* (40,006 votes). Most popular: *Avengers: Endgame*.

**Franchise vs standalone**
- 16 of the 18 movies belong to a franchise; only 2 are standalone.
- The 2 standalone movies show slightly higher mean revenue ($1,765M vs $1,691M), median ROI (9.6 vs 7.8) and rating (7.45 vs 7.38).
- With only 2 standalone movies, this comparison is **not reliable**: one big hit like *Titanic* pulls the average up.

**Franchises and directors**
- *The Avengers Collection* is the most successful franchise: 4 movies, **$7,904M** total revenue and the highest mean rating (7.95).
- *James Cameron* is the most successful director: 2 movies (*Avatar*, *Titanic*), **$5,188M** total revenue.

**What the charts show**
- *Revenue vs Budget:* correlation of 0.58 – bigger budgets tend to earn more, but not always.
- *ROI by Genre:* Romance (9.6), Fantasy (9.1) and Drama (8.9) have the highest median ROI, Action (6.9) the lowest; several genres have only 1–2 movies.
- *Popularity vs Rating:* correlation of 0.77 – better-rated movies are clearly more popular.
- *Yearly box office:* 2015, 2018 and 2019 are highest because they have 3–4 movies each in this list, not because of a market trend.

**Advanced searches**
- Neither search (Sci-Fi Action with Bruce Willis; Uma Thurman directed by Quentin Tarantino) returned results. The code works, but none of the 18 movies matches these conditions.

## 4. Limitations

- **Small dataset:** 18 movies, all very successful blockbusters, so results cannot be generalised to all movies.
- **Only one director per movie:** the code keeps the first "Director" in the crew. Movies with two directors (e.g. the Russo brothers) are split, so Anthony Russo and Joe Russo appear separately.
- TMDB data changes over time; the results reflect the data on 12 October 2026.
- Revenue is not adjusted for inflation, so older movies like *Titanic* (1997) are compared in money of different years.

## 5. Conclusions

- Franchise movies dominate the list of the highest-earning films, and *The Avengers* is the strongest franchise in both revenue and rating.
- Profit and ROI tell a different story from revenue: *Avatar* and *Titanic* earned the most for the money spent.
- High budgets do not guarantee the best return or the best rating.
- To make stronger conclusions, the same pipeline could be run on a larger and more varied set of movies.
