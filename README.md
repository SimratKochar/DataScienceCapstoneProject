# Fandango Ratings Bias Analysis

> An exploratory data analysis investigating whether Fandango — a site that both
> **displays movie ratings** and **sells tickets** — inflates the ratings it
> shows, compared with platforms that only publish reviews (Rotten Tomatoes,
> Metacritic, IMDb).

---

## Overview

**Problem.** If a company shows you a movie's rating *and* profits when you buy a
ticket, can you trust the rating? This project tests, with data, whether Fandango's
displayed ratings are systematically higher than the same films' ratings on
review-only platforms.

**Approach.** Two datasets (Fandango's own scraped ratings, and a cross-site
score table) are cleaned, normalised to a common 0–5 scale, and compared through
distribution plots and per-film differences.

**Result.** Fandango's *displayed* star ratings are consistently higher than both
its own underlying user ratings and the ratings on competing platforms — clear
evidence of upward bias. The starkest example: *Taken 3 (2015)* showed **4.1 stars**
on Fandango while averaging far lower across the other sites.

---

## Data

| File | Rows | Description |
|------|-----:|-------------|
| `fandango_scrape.csv` | ~504 films | Fandango's `STARS` (displayed), `RATING` (actual), and `VOTES` |
| `all_sites_scores.csv` | ~146 films | Rotten Tomatoes (critic + user), Metacritic (critic + user), and IMDb scores, with vote counts |

**Source:** FiveThirtyEight, *"Be Suspicious Of Online Movie Ratings, Especially
Fandango's"* (the data behind that story).

Key cleaning steps in the notebook: stripping the release year out of film titles
into a separate column, removing films with zero votes, and rescaling each
platform's score onto a shared 0–5 range so they're directly comparable.

---

## What the analysis shows

1. **Displayed vs. true (Fandango only).** Comparing the `STARS` shown to users
   against the underlying `RATING` reveals Fandango almost always *rounds up* —
   the displayed star count exceeds the true rating, sometimes by close to a full
   star.
2. **Fandango vs. the rest.** Once all platforms are on the same scale, Fandango's
   ratings sit visibly above Rotten Tomatoes, Metacritic, and IMDb.
3. **Worst offenders.** Films with the largest gap between Fandango and the
   cross-site average (e.g. *Taken 3*) make the inflation obvious.

**Takeaway:** the pattern is consistent with Fandango boosting displayed ratings,
plausibly to drive ticket sales.

---

## Repository structure

```
.
├── Capstone-Project.ipynb     # full analysis notebook
├── fandango_scrape.csv        # Fandango displayed/true ratings + votes
├── all_sites_scores.csv       # cross-platform scores
└── README.md
```

## Setup

```bash
git clone https://github.com/SimratKochar/DataScienceCapstoneProject.git
cd DataScienceCapstoneProject

python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install numpy pandas matplotlib seaborn jupyter

jupyter notebook Capstone-Project.ipynb
```

## Tech stack

`Python` · `pandas` · `NumPy` · `Matplotlib` · `Seaborn` · `Jupyter`

---

## Key learnings & next steps

- Normalising metrics onto a common scale before comparing is essential —
  raw scores across platforms aren't directly comparable.
- The conclusion is descriptive, not yet statistical. **Next steps:** add a formal
  hypothesis test (e.g. a paired test on the per-film Fandango-vs-others
  difference) with effect sizes and confidence intervals, refresh the data with a
  current pull so the finding isn't limited to 2015 films, and correlate displayed
  ratings against ticket-sales data to test the proposed sales-driven motive.

## Acknowledgements

Originally completed as the capstone for the *Python for Machine Learning & Data
Science Masterclass* (Pierian Data) on Udemy. The dataset is from FiveThirtyEight.
