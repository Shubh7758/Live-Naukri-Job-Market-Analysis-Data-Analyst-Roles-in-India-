# Naukri Job Market Analysis — Data Analyst Roles in India

A live scraper that pulls Data Analyst job listings from Naukri.com daily and feeds them into an interactive Power BI dashboard. Built this to understand what the DA job market in India actually looks like right now — which cities are hiring, what skills companies want, and what salary ranges look like across experience levels.

---

## What this does

The scraper runs on a schedule, collects job listings from Naukri, deduplicates them against the existing dataset, and appends only new records. The Power BI dashboard reads from this CSV and gives a live picture of the market across three report pages.

No sample dataset. No Kaggle. The data is pulled fresh from the source.

---

## Dashboard pages

**Job Market Overview**
Total jobs, companies hiring, average salary, posting freshness, and a breakdown by experience level (Fresher / Junior / Senior).

**Location & Salary Intelligence**
City-wise hiring volume, average salary by city and experience level, work mode split (Remote / Hybrid / On-site), and best-paying city benchmarks.

**Skills Intelligence**
Top skills ranked by frequency, skill share percentage, average skills required per listing, and skill demand broken down by experience level.

---

## Screenshots

| Overview | Location & Salary | Skills |
|---|---|---|
| ![Overview](screenshots/overview.png) | ![Location](screenshots/location_salary.png) | ![Skills](screenshots/skills.png) |

---

## Tech stack

| Layer | Tools |
|---|---|
| Scraping | Python, Selenium (headless Chrome), BeautifulSoup4 |
| Data wrangling | Pandas |
| Storage | CSV (incremental append) |
| Visualisation | Power BI, DAX |

---

## Project structure

```
naukri-job-market-analysis/
├── data/
│   └── naukri_jobs.csv          # scraped dataset
├── notebook/
│   └── Naukri_scraper.ipynb     # scraper code
├── dashboard/
│   └── Live_Naukri_Job_Market_Analysis.pbix
├── screenshots/
│   ├── overview.png
│   ├── location_salary.png
│   └── skills.png
└── README.md
```

---

## How the scraper works

Naukri renders job cards via JavaScript, so a simple `requests` call returns an empty page. The scraper uses Selenium with a headless Chrome driver to fully load the page, then passes the rendered HTML to BeautifulSoup for parsing.

For each job card it extracts:

- Job title
- Company name
- Experience required
- Salary (where disclosed)
- Location
- Skills listed
- Date posted
- Timestamp of when the record was scraped

**Incremental loading** — on every run, new records are appended to the existing CSV and deduplicated on `Title + Company`. This means you can run the scraper daily without duplicating data or losing history. The `Scraped_At` field tracks when each record was collected, which lets the Power BI dashboard show posting freshness.

---

## How to run it

**Prerequisites**

```bash
pip install selenium webdriver-manager beautifulsoup4 pandas
```

Chrome must be installed on your machine. The `webdriver-manager` library handles the ChromeDriver download automatically.

**Run the scraper**

Open `notebook/Naukri_scraper.ipynb` in Jupyter and run all cells. By default it scrapes 5 pages (~100 listings per run). Change the `PAGES` variable at the top to scrape more.

```python
PAGES = 5   # increase this to scrape more listings
```

The output is saved to `data/naukri_jobs.csv`.

**Open the dashboard**

Open `dashboard/Live_Naukri_Job_Market_Analysis.pbix` in Power BI Desktop and update the data source path to point to your local `naukri_jobs.csv`.

---

## Key findings

A few things that stood out from the data:

- SQL and Python appear in the majority of listings regardless of experience level — they're non-negotiable
- Bangalore and Hyderabad account for the largest share of DA openings, but Mumbai tends to have higher disclosed salaries
- A significant portion of fresher-level roles don't disclose salary — disclosed salary listings skew toward mid/senior experience
- Remote roles are a small fraction of total openings; most DA roles in India are still hybrid or on-site

---

## What I'd add next

- Schedule the scraper to run daily via GitHub Actions and auto-commit the updated CSV
- Add more fields — job description text, company size, industry
- Build a trend layer to track how skill demand and salary shift week over week

---

## Connect

If you're working on something similar or have feedback on the analysis, feel free to reach out.

[LinkedIn](https://www.linkedin.com/in/shubham-shevare-b04a621a0) · [GitHub](https://github.com/Shubh7758)
