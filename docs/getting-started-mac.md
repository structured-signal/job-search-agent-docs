# Overview of the Job Search Agent

By the end of this article, you'll have this job search agent running and a CSV file of job listings saved to your machine or to Google Docs. The job search agent uses JobSpy. 

## Prerequisites

- Python 3.12 installed
- pip installed
- a terminal

> [!NOTE]
> JobSpy doesn't yet run on Python 3.14. 

## Installation

Install required libraries: 

```bash
python3.12 -m pip install jobspy

```

## Configuration

Open `job_search.py` and edit the search terms to match the roles you're targeting. For example, you might be looking for a technical editor or documentation engineer role. Run this entire file: 

```python

from jobspy import scrape_jobs
from datetime import date
import pandas as pd

search_terms = [
    "AI content strategist",
    "documentation engineer",
    "technical writer AI",
    "technical editor",
    "documentation specialist"
]

all_jobs = []

for term in search_terms:
    for site in ["indeed", "linkedin"]:
        try:
            jobs = scrape_jobs(
                site_name=[site],
                search_term=term,
                location="Remote",
                results_wanted=50,
                hours_old=72,
                country_indeed="USA"
            )
            all_jobs.append(jobs)
        except Exception as e:
            print(f"Skipping '{term}' on {site} due to error: {e}")

if all_jobs:
    results = pd.concat(all_jobs).drop_duplicates(subset=["title", "company"])
    filename = f"ai_jobs_{date.today()}.csv"
    results.to_csv(filename, index=False)
    print(f"Done. {len(results)} jobs saved to {filename}")
else:
    print("No results collected.")

```

## Run jobspy

```bash
python job_search.py
```

> [!NOTE]
> You may need to try `python3.12 job_search.py` or `python3 job_search.py` if you have other versions of Python on your machine.

## Expected output

A file named `ai_jobs_YYYY-MM-DD.csv` saved to your current directory. This file contains deduplicated job listings from Indeed and LinkedIn. LinkedIn may skip some search terms. This is expected behavior.