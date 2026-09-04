# Ola City Ride Issue Analyzer

A Python/Pandas pipeline that merges ride, refund, complaint, ticket, customer, and driver data into a single prioritized operations view — helping support teams triage cancellations, delays, refund breaches, and repeat complaints without cross-referencing seven separate files by hand.

## What it does

- Loads and validates 7 heterogeneous data sources (CSV, Excel, JSON, text)
- Cleans inconsistent statuses, currency-formatted amounts, and malformed dates
- Classifies cancellations by owner (Driver / Customer / System) and reason
- Flags SLA breaches for driver delay, customer wait time, and refund processing
- Aggregates complaints and support tickets to the ride and customer level
- Detects repeat-issue customers across multiple rides
- Assigns a hierarchical issue priority (P0–P3) with a specific recommended action per ride
- Provides an interactive ride-lookup GUI and a filterable operations queue (city, priority, refund status, cancellation owner)
- Includes a baseline linear regression exploring what drives complaint volume
- Exports auditable CSV reports: full master report, P0 critical-rides report, city-wise summary

## Tech stack

Python · Pandas · NumPy · scikit-learn (regression) · ipywidgets (GUI) · Jupyter/Colab

## Project structure

```
Ola_City_Ride_Issue_Analyzer.ipynb   # main notebook — end-to-end pipeline
data/
  rides.csv
  refund_requests.xlsx
  customer_complaints.json
  support_tickets.csv
  customers.csv
  drivers.csv
  city_ops_notes.txt
output/
  final_ride_issue_report.csv        # full master report
  p0_critical_rides_report.csv       # critical rides only
  city_issue_summary.csv             # city-wise rollup
```

## How it works

The notebook runs as a linear pipeline: **load → validate → clean → derive features (cancellations, delays, refunds, complaints, tickets) → merge into a master table → classify priority → generate recommended actions → export reports.** Every fix made to the original broken codebase is logged in a structured debug log inside the notebook, documenting the root cause and resolution for over 20 issues.

## Usage

1. Open `Ola_City_Ride_Issue_Analyzer.ipynb` in Jupyter or Google Colab.
2. Place the input files in a `data/` folder alongside the notebook (or update the paths in Section 1).
3. Run all cells top to bottom.
4. Use the search widget to look up any ride by ID, or call `get_filtered_operations_view()` to pull a work queue by city, priority, refund status, or cancellation owner.
5. Find exported reports in the `output/` folder.

## Limitations

- The regression model is a simple linear baseline for feature exploration, not a production forecasting model.
- SLA thresholds (15-min driver arrival, 10-min customer wait, 7-day refund window) are documented assumptions used where exact values weren't specified.
- The tool supports triage and prioritization; it does not take automated action on tickets or refunds.
