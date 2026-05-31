# Jordan GDP Analysis Dashboard
### Power BI Case Study | Department of Statistics, Jordan

---

## Overview

This project analyzes Jordan's Gross Domestic Product by economic activity (ISIC4 classification) at current prices over the period **2008–2023**, using data published by the Department of Statistics, Jordan. The analysis identifies sector-level GDP trends, highlights underperforming sectors, and delivers three consulting-grade strategic recommendations.

---

## Dataset

| Field | Details |
|---|---|
| Source | Department of Statistics, Jordan |
| File | Nat_01 |
| Unit | Million Jordanian Dinar (JD) |
| Coverage | 2008 – 2023 |
| Classification | ISIC4 (International Standard Industrial Classification, Rev. 4) |
| Prices | Current Prices |
| Note | 2023 figures are Preliminary Annual Estimates |
| FX Reference | 1 JD = 1.41042 USD (fixed peg since 1995) |

---

## Data Preparation

Raw data required significant cleaning before analysis:

- Removed title rows, blank rows, and 23 trailing footnote rows
- Fixed missing column header (`Economic Activity`) and stripped asterisk from 2023 year label
- Stripped leading/trailing whitespace from all sector names
- Restructured from **wide format** (years as columns) to **long format** (one row per sector/year) for Power BI compatibility
- Added `Row_Type` classification column: `Sector` | `GDP Aggregate` | `Adjustment`
- Replaced accounting-style bracket formatting on negative values (FISIM) with standard minus signs
- Moved source metadata to a dedicated `Metadata` sheet

Cleaned file: `Nat_01_PowerBI_ready.xlsx` — 384 rows × 4 columns

---

## DAX Measures

```dax
-- Previous Year GDP (integer year compatible)
GDP_PrevYear =
CALCULATE(
    SUM('Data'[Value_JD_Millions]),
    FILTER(ALL('Data'), 'Data'[Year] = MAX('Data'[Year]) - 1)
)

-- Year-over-Year Growth %
YoY_Growth% =
DIVIDE(
    SUM('Data'[Value_JD_Millions]) - [GDP_PrevYear],
    [GDP_PrevYear],
    0
) * 100

-- CAGR 2008–2023
CAGR =
VAR StartValue = CALCULATE(SUM('Data'[Value_JD_Millions]), 'Data'[Year] = 2008)
VAR EndValue   = CALCULATE(SUM('Data'[Value_JD_Millions]), 'Data'[Year] = 2023)
VAR Years = 15
RETURN (DIVIDE(EndValue, StartValue) ^ (1 / Years) - 1) * 100
```

---

## Dashboard Structure

### Page 1 —  Sector Analysis
- Sector-level breakdown with year slicer
- Interactive filtering by economic activity

### Page 2 — Overview
- GDP growth trend line (2008–2023)
- KPI cards: 2023 GDP, Previous Year GDP, YoY Growth %
- Top 10 sectors by total contribution (bar chart)
- Top 5 lowest-contributing sectors (pie chart)

### Page 3 — Recommendations
- Three consulting-grade strategic recommendations for underperforming sectors

---

## Key Findings

| Metric | Value |
|---|---|
| GDP at Market Prices (2023) | 36,273M JD |
| GDP at Market Prices (2008) | 16,080M JD |
| Total Growth (2008–2023) | ~125% |
| CAGR | ~5.5% |
| YoY Growth (2022–2023) | 4.77% |
| COVID impact | GDP contracted in 2020, recovered by 2021 |
| Top sector | Manufacturing (6,270M JD in 2023) |

---

## Strategic Recommendations

### 1. Water Supply & Sewerage
**Root Cause:** Heavy state subsidization, no private investment, import dependency.
**Recommendation:** Expand Red Sea desalination at Aqaba via public-private partnerships; develop domestic bottled water manufacturing to reduce import reliance.
**Impact:** Grows Water Supply and Manufacturing sectors simultaneously; eligible for World Bank and GEF climate financing.

### 2. Arts, Entertainment & Recreation
**Root Cause:** Creative output exists but lacks monetization infrastructure and export pathways.
**Recommendation:** Position Jordan as an Arabic content production hub for MENA streaming platforms; develop cultural tourism packages around Petra, Jerash, and Wadi Rum to convert heritage assets into arts sector revenue.
**Impact:** Links arts growth to existing tourism GDP; leverages Jordan's UNESCO heritage sites and regional soft power.

### 3. Administrative & Support Services
**Root Cause:** Sector is domestically focused; Jordan has not captured regional BPO opportunity despite superior bilingual human capital.
**Recommendation:** Launch a national BPO strategy targeting Arabic-language services for Gulf corporates and INGOs; offer investment incentives for shared service centers in Amman.
**Impact:** Converts underemployed graduate talent into export revenue; requires no physical infrastructure investment.

---

## Tools & Skills Used

- **Python** — data cleaning, reshaping, Excel output (openpyxl, pandas)
- **Power BI Desktop** — data modeling, DAX measures, dashboard design
- **DAX** — YoY growth, CAGR, dynamic previous-year calculation
- **Excel** — intermediate cleaned file with metadata sheet

---

## Files

| File | Description |
|---|---|
| `Nat_01_20260524.xlsx` | Raw source data |
| `Nat_01_Cleaned_DateFixed.xlsx` | Wide format, cleaned and formatted |
| `Nat_01_PowerBI_ready.xlsx` | Long format, analysis-ready |
| `Jordan_GDP_Theme.json` | Custom Power BI theme (Jordan national colors) |
| `GDP ANALYSIS.pbix` | Power BI dashboard file |
| `README.md` | This file |

---

## Author

**Seleen Eshtayah**
Physics BSc, University of Jordan
[linkedin.com/in/seleen-eshtayah](https://linkedin.com/in/seleen-eshtayah) | [github.com/SeleenAhmad](https://github.com/SeleenAhmad) | seleeneshtayah@gmail.com
