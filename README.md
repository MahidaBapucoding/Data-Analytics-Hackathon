# VoltRelay Energy — Data Analytics Hackathon

## Project Overview

VoltRelay operates a battery-swapping network. This project analyzes supplied hackathon data to describe network growth, service reliability, station and battery patterns, rider retention, and supported transaction economics. Results are descriptive; observational associations are not presented as causal effects.

## Analytical Approach

The notebook validates source schemas and join keys, standardizes city labels, corrects the documented firmware timestamp window, and streams the large swap-event CSV. It covers:

- Network performance over time
- Service failures and station-hour telemetry
- Station and geographic comparisons
- Delivered battery state-of-health cohorts
- Pricing, discounts, and fleet-partner economics
- Rider retention with full-follow-up windows

## Key Findings

All figures below are calculated in the final analysis report. Differences are observational and do not establish causes.

- In June 2025, there were 331,719 attempts and 303,196 completed swaps; the observed failure rate was 5.6%.
- City-tagged events had a 5.1% observed failure rate in Jaipur and 2.7% in Mumbai. These city differences do not isolate operating causes.
- In station-hours with calculable charged-stock/target ratios, the lowest quartile had a 4.3% failure rate (434,080 attempts) and the highest quartile had a 3.5% rate (389,238 attempts). The comparison covers 464,818 of 1,026,438 eligible ok/partial station-hours; telemetry-missing and stock-unavailable hours are excluded. This is an association, not proof that stock caused failures.
- In the same-pack, same-age 2W_2.1kWh cohort aged 6–12 months, delivered SOH averaged 80.5% for Kyron and 89.5% for Cellora. This cohort comparison does not establish supplier causation.
- Thirty-day return was 99.0% after a rider's first attempt failed (n=1,024) and 99.6% without a first-attempt failure (n=17,669), an unadjusted 0.6 percentage-point difference. Return was high in both groups.
- Across 3,586,675 completed swaps, observed charges totaled ₹229,195,296, or ₹64 per completion. Recorded discounts totaled ₹20,875,332. The estimated energy-only contribution proxy was ₹44 per completion; it is not contribution margin because fixed station costs, labor, fees, battery depreciation, and other costs are excluded.

## Recommendations

- Review higher-rate city stations against queue, charged inventory, and outage conditions before changing capacity.
- Inspect STN-DEL-037's matched telemetry and operating hours; validate local demand and stock before a station-specific pilot.
- Check inventory timing in low charged-stock/target station-hours before targeted stock changes; monitor matched-hour failure rate, queue wait, and stock ratio.
- Audit SOH readings and manufacturing lots for the same-pack, same-age battery cohort, then verify against usage and maintenance records before supplier action.
- Review first-attempt failures in adjusted cohorts before service-policy changes and track eligible 30-day return.
- Compare recorded partner discount amounts, charged revenue, and contract terms before partner pricing changes.

These are review priorities grounded in observed patterns, not proven interventions.

## Repository Structure

```text
.
├── VoltRelay_Data_Analytics.ipynb
├── build_notebook.py
├── requirements.txt
└── outputs/
    ├── VoltRelay_Analysis_Report.md
    ├── video_script_3min.md
    ├── linkedin_post.md
    └── SUBMISSION_CHECKLIST.md
```

The notebook creates an `outputs/` folder for generated analysis tables when run. Generated CSV tables are not committed.

## How to Run

1. Install the packages in `requirements.txt` in a Python environment with Jupyter support, or open the notebook in Google Colab. `nbformat` is included for the optional notebook-builder script.
2. Obtain the eight official hackathon CSVs and place them in the same folder as the notebook (or upload all eight to the Colab runtime's `/content` folder). Keep the exact filenames listed below. The notebook defaults to `DATA_DIR = Path(".")`; edit that one setting if the CSVs are elsewhere.
3. Open `VoltRelay_Data_Analytics.ipynb`.
4. Restart the runtime/kernel and run all cells from top to bottom. The event file is large; allow time and memory for the chunked analysis.

To regenerate the notebook structure from its source script, run `python build_notebook.py`.

Expected files: `swap_events.csv`, `station_hourly_status.csv`, `riders.csv`, `batteries.csv`, `support_tickets.csv`, `stations.csv`, `city_daily_context.csv`, and `fleet_partners.csv`.

## Data Availability

Raw hackathon datasets are intentionally excluded from this public repository. Obtain them from the official hackathon dataset source. The analysis notebook expects the eight CSVs listed above and does not include or redistribute them.
