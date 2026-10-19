# 🎬 Investigating the TMDb Movie Dataset: Drivers of Box Office Revenue & Genre Trends

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Wrangling-150458?logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-Vectorized%20Math-013243?logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Visualization-11557c)
![Seaborn](https://img.shields.io/badge/Seaborn-Statistical%20Plots-4c72b0)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

## 📌 Project Overview
This project conducts an end-to-end Exploratory Data Analysis (EDA) on **The Movie Database (TMDb)** dataset, containing metadata and financial records for over 10,000 films released between 1960 and 2015. Using Python (`pandas`, `NumPy`, `matplotlib`, and `seaborn`), this investigation cleans raw box-office records, handles multi-valued pipe-separated features, and uncovers the key movie characteristics and genre dynamics associated with high inflation-adjusted box office revenue.

---

## 🔍 Research Questions
In this analysis, **Inflation-Adjusted Revenue (`revenue_adj`, in 2010 USD)** serves as the primary dependent variable, evaluated against key independent variables (`budget_adj`, `vote_average`, `runtime`, `genres`, and `release_year`):

1. **Movie Properties & Financial Performance:** Which movie properties (`budget_adj`, `vote_average`, and `runtime`) are most strongly associated with high inflation-adjusted box office revenue (`revenue_adj`)?
2. **Genre & Temporal Dynamics:** Which genres are the most frequently produced, which generate the highest average inflation-adjusted revenue, and how have movie production volume and box office revenue evolved over time (`release_year`)?

---

## 🛠️ Data Wrangling & Cleaning Pipeline
The raw dataset (`10,866 rows × 21 columns`) required systematic preprocessing to ensure analytical integrity:
* **Dimensionality Reduction:** Removed 10 non-analytical metadata/identifier columns (`id`, `imdb_id`, `homepage`, `tagline`, `keywords`, `overview`, `cast`, `director`, `production_companies`, `release_date`).
* **Deduplication & Null Handling:** Eliminated exact duplicate records and removed rows missing critical categorical labels (`genres`) or valid durations (`runtime`).
* **Handling Unreported Financials:** Over 50% of the raw records listed `0` for budget (`5,696` rows) and revenue (`6,016` rows)—representing unrecorded data rather than $0 films. Zeroes were converted to `NaN` via vectorized NumPy operations, yielding two clean analytical views:
  * **`df_clean` (`10,812` movies):** Retained for overall production volume, runtime, and genre frequency trends.
  * **`df_financial` (`3,854` movies):** Filtered for verified positive (`> 0`) inflation-adjusted budget and revenue figures.
* **Multi-Label Feature Explosion:** Built a reusable `expand_pipe_column()` helper function to split and explode pipe-delimited (`|`) genre strings into atomic rows for accurate aggregation.

---

## 📊 Key Findings & Insights

### 1. Drivers of Box Office Revenue
* **Production Budget (`budget_adj`):** Exhibits the strongest positive linear correlation with inflation-adjusted revenue ($r = 0.57$). Larger budgets support wider theatrical distribution, star talent, and major marketing campaigns.
* **The Budget × Rating Synergy (`vote_average`):** While user ratings alone show a moderate correlation with revenue ($r = 0.27$), multi-variable tier analysis (`pd.cut` and `pd.qcut`) reveals a strong interaction effect. **High-Budget films** that achieve **Acclaimed ratings (>7.5)** generate an average of **>$450M (2010 USD)**—more than double the revenue of poorly rated high-budget releases.
* **Runtime (`runtime`):** Shows a positive association with revenue ($r = 0.28$), with top-grossing blockbusters clustering between **110 and 160 minutes**.

### 2. Genre Volume vs. Box Office Returns
* **Most Produced Genres (Volume):** **Drama** (4,754 films), **Comedy** (3,782 films), and **Thriller** (2,904 films) dominate global film production.
* **Highest-Grossing Genres (Average Revenue):** High production volume does not translate to the highest per-film earnings. Spectacle-driven genres lead in mean inflation-adjusted revenue:
  1. **Adventure:** $271.40M *(Mean Adjusted Budget: $81.50M)*
  2. **Animation:** $252.95M *(Mean Adjusted Budget: $83.47M)*
  3. **Fantasy:** $249.99M *(Mean Adjusted Budget: $80.24M)*
  4. **Science Fiction:** $202.11M *(Mean Adjusted Budget: $65.62M)*

### 3. Historical Evolution (1960–2015)
* **Exponential Production Growth:** Annual film output rose from fewer than 50 releases/year in the 1960s to nearly **700 releases/year by 2014–2015**, driven by digital filmmaking and expanded distribution channels.
* **Stabilization of Mean Revenue:** Average inflation-adjusted revenue per film stabilized between **$110M and $150M** from the 1990s onward as mid- and low-budget independent films were cataloged alongside major studio blockbusters.

---

## ⚠️ Limitations
1. **Survivorship / Reporting Bias:** Filtering out unrecorded (`$0`) financial rows reduced the financial sample from 10,812 to 3,854 films, skewing financial insights toward commercially tracked studio releases.
2. **Equal Genre Attribution:** Exploding multi-genre films (e.g., `Action|Adventure|Sci-Fi`) attributes a movie's full budget and revenue equally to each listed genre rather than weighting by primary genre.
3. **Omitted Ancillary & Marketing Streams:** The dataset captures production budgets and theatrical box office receipts but excludes print-and-advertising (P&A) spend, streaming licensing, and merchandising.

---

## 📁 Repository Structure
```text
├── Database_TMDb_movie_data/
│   └── tmdb-movies.csv             # Raw TMDb dataset (10,866 movies)
├── Investigate_a_Dataset.ipynb     # Main Jupyter Notebook with code, EDA, and visualizations
├── Investigate_a_Dataset.html      # Exported static HTML report for quick browser viewing
├── requirements.txt                # Python package dependencies
└── README.md                       # Project documentation
