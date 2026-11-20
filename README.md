[![Oportunitati si Cariere](https://github.com/peviitor-scrapers/netpro-srl-python-scraper/actions/workflows/job-seeker-ro-spider.yml/badge.svg)](https://github.com/peviitor-scrapers/netpro-srl-python-scraper/actions/workflows/job-seeker-ro-spider.yml)
[![Automation Tests](https://github.com/peviitor-scrapers/netpro-srl-python-scraper/actions/workflows/automation-testing.yml/badge.svg)](https://github.com/peviitor-scrapers/netpro-srl-python-scraper/actions/workflows/automation-testing.yml)
[![Version](https://img.shields.io/badge/version-1.0.0-blue)](CHANGELOG.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.12-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Website](https://img.shields.io/website?url=https%3A%2F%2Fpeviitor.ro&label=peviitor.ro)](https://peviitor.ro)
[![API](https://img.shields.io/website?url=https%3A%2F%2Fapi.peviitor.ro%2F&label=api.peviitor.ro)](https://api.peviitor.ro/)
[![GitHub Pages](https://img.shields.io/github/deployments/peviitor-scrapers/netpro-srl-python-scraper/github-pages?label=GitHub%20Pages)](https://peviitor-scrapers.github.io/netpro-srl-python-scraper/)

# job_seeker_ro_spider — NETPRO SRL Scraper

**job_seeker_ro_spider** — a scraper for NETPRO SRL jobs in Romania
(CUI `13390452`, Oradea). It collects the active postings published on
[ANOFM](https://mediere.anofm.ro/app/module/mediere/jobs),
[jobRapid](https://www.jobrapid.ro/), [BeBee](https://bebee.com/),
[Jobica](https://jobica.ro/), [Emprego](https://emprego.com/),
[Cauta.ro](https://cauta.ro/) and [JobRadar24](https://www.jobradar24.ro/),
then publishes them to
[peviitor.ro](https://peviitor.ro) through the Peviitor API.

ANOFM is the authoritative source (`employer_tax_code=13390452`). The portals
republish the same offer on their own detail pages; because peviitor.ro
identifies jobs by unique URL, each distinct URL is published as a distinct job.
JobRadar24 omits the employer name in JSON-LD, so its company is verified from
the page text.

> **🌱 Derived scraper.** This repository is derived from
> [palas-international-resort-python-scraper](https://github.com/peviitor-scrapers/palas-international-resort-python-scraper).

## Overview

The project automates the daily collection of NETPRO SRL jobs in
Romania, keeping the peviitor.ro board up to date with the latest opportunities.

## Features

- Extracts jobs from ANOFM (`mediere.anofm.ro`) by employer CUI
- Extracts jobRapid postings by company-name search and JSON-LD `JobPosting` blocks
- Derives BeBee and Jobica detail URLs from ANOFM ids, maps the Cauta.ro
  company profile and follows the discovered JobRadar24 detail URLs; the
  hiring organization is verified on every page
- Deduplicates by URL across all seven sources; distinct URLs remain distinct jobs
- Publishes enriched non-personal fields: location, salary, date and expiry date
- Never stores job descriptions, contact names, emails or phone numbers (GDPR data minimization)
- Validates the company via CUIScan (legal name and address, by CIF) and CUIFirma (brand search)
- **Company cache** — does not hit the APIs on every scrape
- **Stale cache / config fallback** when CUIScan is unavailable
- Cross-validates against the Peviitor API
- Deletes stale jobs from peviitor when they are no longer live (scoped to this scraper's URL prefixes)
- Stores to the Peviitor API (job core + company core)
- Generates `docs/jobs.md` automatically — accessible on GitHub Pages
- **Company identity in a single file** (`scraper/config/company.json`)
- GitHub Actions: daily scrape + automated testing (unit, integration, e2e, consistency)
- Identifies itself through the User-Agent: `job_seeker_ro_spider`

## Local setup

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m pytest tests/unit tests/consistency   # offline checks
.venv/bin/python -m scraper.index                         # full scrape + publish
```

## License

Copyright (c) 2026 Alexandra Ifrim

Licensed under the [MIT License](LICENSE).

## Managed By

This project is managed by [ASOCIATIA OPORTUNITATI SI CARIERE](https://oportunitatisicariere.ro) and used as a web scraper for the [peviitor.ro](https://peviitor.ro) job board project.

## Disclaimer

This scraper is designed for educational purposes and legitimate job data aggregation for the Romanian job market.
