# Investment Portfolio Tracker

> Interactive multi-investor portfolio dashboard built in Power BI | Star Schema · DAX Measures · Cross-Filtering Slicers

---

## What This Is

A portfolio analyst's daily questions are simple — how much have I invested, what is it worth now, which assets are working and which are not. The hard part is building something that answers all of those questions instantly, for any investor, any asset type, any time period, without rebuilding the report every time.

This project does exactly that. The dataset was designed from scratch in Excel as a proper relational star schema, imported into Power BI, and powered by DAX measures that respond dynamically to every slicer selection. The result is a single-page dashboard where every number updates in real time.

---

## Dashboard

**One page. Six visuals. Four slicers. Everything connected.**

| Section | Visual | Purpose |
|---|---|---|
| Top | KPI Cards | Total Investment · Current Value · Total Return · Return % · Top Asset |
| Middle Left | Pie Chart | Current portfolio value by asset type |
| Middle Right | Donut Chart | Original investment allocation by asset type |
| Bottom Left | Line Chart | Portfolio value trend over time by month |
| Bottom Centre | Clustered Column | Total return comparison by asset type |
| Bottom Right | Pivot Table | Position-level detail — name, qty, cost, value, return % |

---

## Data Model — Star Schema

The foundation was built in Excel before importing into Power BI. A star schema separates transactional data from descriptive attributes — this is what makes cross-filtering work correctly without double counting.

```
        Dim_Investor
             │
             │
Dim_Date ── Fact_Investments ── Dim_Asset
```

| Table | Type | Key Fields |
|---|---|---|
| Fact_Investments | Fact | Invested_Value, Current_Value, Quantity, Return_% |
| Dim_Asset | Dimension | Asset_ID, Asset_Name, Asset_Type |
| Dim_Investor | Dimension | Investor_ID, Investor_Name, Risk_Profile |
| Dim_Date | Dimension | Date, Month, Month_Year, Quarter, Year |

---

## DAX Measures

Five measures written to power the dashboard — all context-aware, all responding correctly to any slicer combination.

```DAX
TOTAL INVESTMENT = SUMX(Fact_Investments, Fact_Investments[Invested_Value])

TOTAL CURRENT VALUE = SUMX(Fact_Investments, Fact_Investments[Current_Value])

TOTAL RETURN = [TOTAL CURRENT VALUE] - [TOTAL INVESTMENT]

TOTAL RETURN % = DIVIDE([TOTAL RETURN], [TOTAL INVESTMENT])

TOP PERFORMING ASSET = 
    RANKX(ALL(Dim_Asset[Asset_Type]), [TOTAL RETURN], , DESC)
```

**Why DIVIDE instead of division operator?**
DIVIDE handles zero and blank denominators gracefully — no errors when a slicer filters to an empty result set.

**Why SUMX instead of SUM?**
SUMX iterates row by row — more explicit and reliable when the fact table has multiple rows per asset or investor.

**Why RANKX for top performer?**
RANKX recalculates under any filter context — the top performing asset updates correctly whether you're looking at one investor or all of them.

---

## Slicers

Four slicers — all cross-filtering all visuals simultaneously.

| Slicer | Filters By | Use Case |
|---|---|---|
| Investor Name | Individual investor | View a single investor's complete portfolio |
| Month | Calendar month | Snapshot the portfolio at any point in time |
| Asset Type | Asset category | Focus on equity, bonds, gold etc. |
| Risk Profile | Investor risk category | Compare Conservative vs Aggressive returns |

The Investor + Risk Profile combination is particularly useful — filter to Aggressive investors and compare returns across asset types to see whether higher risk is actually being rewarded.

---

## Skills Demonstrated

### Power BI
- Star schema data model with defined table relationships — not a flat file import
- DAX measure writing — SUMX, DIVIDE, RANKX with filter context awareness
- Cross-filtering slicers controlling all visuals simultaneously
- Visual type selection based on analytical purpose — not aesthetics
- Single-page dashboard layout with information hierarchy from summary to detail
- Dynamic KPI cards including derived text output (top performing asset name)

### Data Preparation
- Designed relational dataset from scratch in Excel — no sample data
- Consistent naming conventions — Fact_ and Dim_ prefixes throughout
- Clean data types before import — dates, numbers and text correctly formatted
- Separation of transactional data from descriptive attributes

### Analytical Thinking
- Pie vs Donut side by side — deliberately shows investment allocation vs current value allocation simultaneously, making portfolio drift instantly visible
- Line chart on date hierarchy — enables month-level drill-down on time axis
- Holdings pivot table — bridges high-level summary with position-level accountability

---

## File Structure

```
📁 Investment-Portfolio-Tracker
├── PORTFOLIO_TRACKER.pbix        # Power BI dashboard file
├── Portfolio_Dataset.xlsx        # Source data — star schema tables
└── Portfolio_Tracker_Report.docx # Full project report
```

---

## How to Use

1. Download `Portfolio_Dataset.xlsx` and `PORTFOLIO_TRACKER.pbix`
2. Open the `.pbix` file in Power BI Desktop
3. Go to **Transform Data → Data Source Settings**
4. Update the file path to point to your local `Portfolio_Dataset.xlsx`
5. Click **Refresh** — dashboard loads with your local data ✅

---

## Author

**Abhinav Saharan**
FRM Candidate | B.Com + MA Economics
*Risk Analyst Portfolio Project — March 2026*

---

## License

MIT License — free to use with attribution.
