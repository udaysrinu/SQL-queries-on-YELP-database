# SQL-queries-on-YELP-database

Profiling and analyzing the Yelp Open Dataset with SQL — Coursera worksheet
submission.

![Yelp dataset ER diagram](yelp%20dataset.png)

## What this is

2020 college-era practice work from the Coursera course *"SQL for Data
Science Capstone"* / *"Data Wrangling, Analysis and AB Testing with SQL"*
(Coursera certificate `ENWWXQ9L72VF` is checked in as PDF). It's an
assignment, not a product — submitted, peer-reviewed, archived here.

The whole submission lives in a single text file:
[`SQl yelp dataset assignment.txt`](./SQl%20yelp%20dataset%20assignment.txt).
Each numbered question contains the SQL I ran and the result table I pasted
back from the Coursera SQL console.

## The dataset

Coursera shipped a 10k-row-per-table sample of the
**[Yelp Open Dataset](https://www.yelp.com/dataset)** — Yelp Inc.'s public
academic dataset of businesses, reviews, users, tips, check-ins, photos, and
attributes. Around 2020 this was a standard playground for SQL / data
science coursework.

Tables in the sample: `attribute`, `business`, `category`, `checkin`,
`elite_years`, `friend`, `hours`, `photo`, `review`, `tip`, `user`.

To reproduce locally, download the full dataset from yelp.com/dataset, load
the JSON into Postgres / MySQL / SQLite, and adjust schema accordingly —
my queries assume the Coursera table names above.

## What's in the queries

**Part 1 — Profiling (questions 1–10):**

- Row counts and distinct-key counts across every table.
- NULL audit on the `user` table.
- `MIN` / `MAX` / `AVG` on review stars, business stars, tip likes, checkin
  counts, user review counts.
- City-level aggregations: cities ordered by total reviews; star-rating
  distributions per city (Avon, Beachwood).
- Top-N users by `review_count` and by `fans`.
- `LIKE`-based text search to compare `"love"` vs `"hate"` review counts
  (1780 vs 232).

**Part 2 — Inferences (open-ended questions):**

- Joins across `business` × `category` × `hours` to compare 2–3 star vs 4–5
  star Toronto food businesses by hours, review counts, and location.
- Open-vs-closed business comparison via `business` × `review`.
- A small cuisine-popularity study: grouping by category
  (`Chinese`, `Mexican`, `Korean`, `French`, `Italian`, `Japanese`, `Indian`),
  computing `COUNT`, `AVG(stars)`, `AVG(review_count)`.

Mostly `GROUP BY` aggregations and `INNER JOIN`s — no CTEs or window
functions. That matches the course scope at the time.

### Findings worth noting

- Las Vegas dominates total reviews (≈82k in the sample).
- More reviews **does not** correlate with more fans — Amy (609 reviews,
  503 fans) outranks Gerald (2000 reviews, 253 fans).
- Korean restaurants had the highest average stars (4.5) in the cuisine
  comparison, though sample sizes are tiny (n=7).

## How to use

1. Get the Yelp Open Dataset (or the Coursera sample if you still have access).
2. Load it into SQLite / Postgres / MySQL.
3. Open `SQl yelp dataset assignment.txt` and run the queries against
   your loaded schema.

## What I'd do differently today

- **Move it to a Jupyter notebook** with pandas + matplotlib / seaborn —
  the cuisine and city comparisons are crying out for bar charts.
- **Use CTEs and window functions** instead of nested subqueries; some of
  the rank-style "top N per group" answers would be cleaner.
- **Verify the "love vs hate" finding** — naive `LIKE "%love%"` matches
  *"glove"*, *"clove"*, *"loved nothing"*. A real analysis needs tokenization
  and sentiment, not substring matches.
- **Sanity-check sample sizes** before drawing inferences — `n=7` Korean
  restaurants is not a popularity signal.
- **Split into one `.sql` file per question** with comments, instead of one
  big text dump.

## License

No license file. Coursework artifact; reference only.
