# LDS Dashboard

A usage statistics dashboard for the **Legal Description Schedule (LDS)** tool. The dashboard is automatically generated nightly from tool usage logs stored in S3-compatible object storage and published as a static site via GitHub Pages.

## Live Dashboard

🔗 **[View Dashboard](https://bcgov.github.io/lds-dashboard/)**

## What It Tracks

- **Usage Volume** — daily run trends, runs by region, GIS vs Non-GIS user breakdown
- **Performance & Reliability** — median LDS/AST processing times, success and failure rates, weekly failure rate trends, common error messages
- **Feature Adoption** — usage rates for optional features like inset maps, provincial reference maps, AST, and legal descriptions

## How It Works

```
S3 Object Storage          GitHub Actions (nightly)         GitHub Pages
┌──────────────┐          ┌─────────────────────┐          ┌──────────────┐
│  JSONL logs  │──read──▶ │  Python script      │──push──▶│  Static HTML │
│  (summary +  │          │  generates dashboard│          │  served at   │
│   detail)    │          │  HTML with Plotly   │          │  public URL  │
└──────────────┘          └─────────────────────┘          └──────────────┘
```

## Data Sources

The dashboard reads two types of monthly JSONL log files from the NRS ObjectStore:

| File pattern | Description |
|---|---|
| `*_summary.jsonl` | One record per LDS run — status, duration, user, region, feature flags |
| `*_detail.jsonl` | Stage-level records per run — used to enrich error messages and compute AST duration |

## Repository Structure

```
├── .github/workflows/
│   └── update_dashboard.yml   # Nightly GitHub Actions workflow
├── lds_usage_dashboard.py     # Dashboard generator script
├── requirements.txt           # Python dependencies
└── README.md
```