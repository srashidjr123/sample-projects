# Sample Projects

Six data-engineering and analysis projects I built while at the
[National Association of Manufacturers](https://www.nam.org/) (NAM).
Each one shipped a public-facing artifact — a live map, a cited report figure,
or a recurring data pipeline that another product depends on.

All notebooks here are sanitized exports of the Databricks notebooks I authored
and operated in production. Credentials, internal paths, and any secrets have
been redacted. Cell outputs have been stripped. Public URLs, agency dataset
sources, and the structural logic of the pipelines are preserved so a reader
can see exactly how the work fits together.

---

## What's in here

| # | Project | What it produces | Live link |
|---|---|---|---|
| 1 | **Bureau of Economic Analysis state map** | The interactive state / county / congressional-district map on the NAM "Manufacturing in the U.S." page. Pulls GDP, employment, and establishment counts from BEA and BLS, regenerates the SVG, and pushes JSON + an inline SVG partial to the live WordPress site. | [nam.org/mfgdata](https://www.nam.org/mfgdata) |
| 2 | **Trade importer** | The interactive trade map on the NAM "Manufactured Goods Trade" page. Ingests HTS-level exports/imports from USA Trade Online, joins to state crosswalks and an end-use code map, and produces the per-state JSON the map consumes. | [nam.org/trade](https://www.nam.org/trade) |
| 3 | **Infrastructure analysis** | The interactive infrastructure map on the "Building to Win" campaign page, plus the freight-congestion cost-to-manufacturers figure used in the campaign narrative. Built on the FHWA Freight Analysis Framework (FAF5) and the National Performance Management Research Data Set. | [nam.org/building-to-win](https://www.nam.org/building-to-win) |
| 4 | **EPA permitting analysis** | The cost-of-permitting-to-manufacturers figure cited in a widely-publicized NAM report. Cleans and joins EPA ECHO air / water / hazardous-waste facility datasets to NAICS-coded manufacturing establishments to size the regulatory footprint. | (cited in NAM permitting report) |
| 5 | **FactoryFix jobs pipeline** | A recurring pipeline that ingests the FactoryFix manufacturing-careers XML feed, geocodes employer addresses, segments listings, and SFTPs the result to the Creators Wanted careers WordPress site. Feeds project 6. | (feeds [creatorswanted.org](https://www.creatorswanted.org/)) |
| 6 | **Skilled-trades credential pipeline** | Builds a unified Delta table of sub-baccalaureate trades credentials (IPEDS) and active manufacturing apprenticeships (DOL RAPIDS) at the state-year grain. Downstream of project 5 for jobs context. | (internal data product) |

---

## Why I picked these for an IFP application

IFP's "Member of Technical Staff" role is about giving a small policy team
leverage — building tools that take policy research and rapidly turn it into
maps, microsites, datasets, and reports. Every project in this repo is an
example of exactly that, scoped down to a one-person team:

- I picked the dataset, owned the ETL, designed the visualization, generated
  the deployable artifacts (SVG, JSON, CSV), and operated the deploy pipeline
  to a WordPress production site.
- Two of the analyses (the **EPA permitting** figure and the **freight
  congestion** figure) became headline numbers in widely-cited NAM
  reports. I used Claude extensively as a thinking partner on both.
- The **trades-credential pipeline** is the most recent and the cleanest
  example of how I write Spark pipelines now — Delta tables, typed UDFs,
  fall-back logic, and config at the top.

I'd hope to bring this same end-to-end ownership to IFP: research → data →
artifact → deployed product, with the engineering hygiene to keep it
maintainable.

---

## Repo layout

```
sample-projects/
├── README.md                              ← this file
├── LICENSE
├── requirements.txt
├── .gitignore
├── 01-bea-state-map/
│   ├── README.md
│   ├── bureau_of_economic_analysis.ipynb
│   └── data/
│       └── country_map_txt.txt            ← SVG template (extracted from notebook)
├── 02-trade-importer/
│   ├── README.md
│   └── trade_importer.ipynb
├── 03-infra-building-to-win/
│   ├── README.md
│   └── infra.ipynb
├── 04-epa-permitting-cost/
│   ├── README.md
│   └── epa_permitting.ipynb
├── 05-factoryfix-jobs-pipeline/
│   ├── README.md
│   └── factoryfix_jobs.ipynb
└── 06-trades-credential-pipeline/
    ├── README.md
    └── trades_credential_pipeline.ipynb
```

---

## A note on sanitization

These notebooks originally ran in Databricks, against Spark and DBFS-mounted
data. To make them readable without a Databricks workspace I:

- Stripped all cell outputs and execution counts.
- Replaced hardcoded credentials (SFTP passwords, API keys, WordPress
  import-trigger keys) with `os.environ[...]` references annotated
  `# REDACTED`.
- Rewrote internal paths — `/dbfs/mnt/dbdata/...` and `/mnt/sandbox/...` —
  to `${DATA_DIR}/...` and `${WORK_DIR}/...` placeholders so the structure
  of the pipeline is still legible.
- Left Spark calls, `dbutils` calls, and the overall control flow intact.
  These notebooks aren't meant to be run standalone — they're meant to be
  *read* as a portfolio of what the work looked like.

---

## Tools

Built on Databricks with PySpark, Pandas, GeoPandas / Shapely, BeautifulSoup,
Plotly, Folium, Paramiko / pysftp, and the `requests` ecosystem. Modeling and
analysis were assisted by Claude in two of the projects (EPA permitting,
freight congestion) — used as a thinking partner for question scoping,
data-cleaning approaches, and figure framing.

---

## Contact

Shareq Rashid · [GitHub @srashidjr123](https://github.com/srashidjr123)
