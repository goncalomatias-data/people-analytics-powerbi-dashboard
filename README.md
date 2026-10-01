# 📊 People Analytics & Internal Equity Dashboard | Power BI

[![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=flat&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/Language-DAX-blue)](https://docs.microsoft.com/dax/)
[![Data Model](https://img.shields.io/badge/Model-Star_Schema-brightgreen)](#data-architecture--modeling)

---

## 📌 Executive Summary
This Power BI reporting solution provides HR leadership and department directors with actionable visibility into **workforce demographics, talent distribution, retention risks, and internal compensation equity**. 

Rather than relying on unstandardized external market benchmarks, the dashboard evaluates salary positioning directly against **internal pay grade midpoints**, enabling granular talent governance and mitigating pay disparities across organizational tiers.

---

## 🖼️ Dashboard Preview

![Dashboard Overview](dashboard_overview.png)

---

## 🎯 Key Business Problems Solved
* **Internal Pay Equity & Compa-Ratio:** Evaluated employee compensation relative to internal pay band medians to pinpoint underpaid high-performers and mitigate attrition risk.
* **Talent Matrix Segmentation:** Mapped employee performance against potential across business units to identify key-talent clusters and promotion priorities.
* **Demographics & Retention Governance:** Tracked headcounts, turnover rates, and age distribution across departments to support workforce and succession planning.
* **Modular Decision-Making:** Designed a clean, accessible UI allowing stakeholders to drill down by department and demographic attributes.

---

## 🏗️ Data Architecture & Modeling
The semantic model is engineered strictly following the **Star Schema (Kimball methodology)**, decoupling transactional facts from descriptive dimensions to optimize VertiPaq engine performance and ensure filter integrity.

![Data Model](data_model.png)

* **Fact Table:** `Fact_Performance`
* **Dimension Tables:** `Dim_JobRole`, `Dim_Employee`
* **Measure Repository:** `_Measures`

---

## 📐 Semantic Modeling & Key DAX Architecture

All business logic is isolated within a dedicated `_Measures` table. Key calculations leverage row-by-row iteration across dimensions and explicit filter contexts:

* **Core Volume & Demographics:** `[Total Headcount]`, `[Average Age]`
* **Talent & Mobility:** `[Avg Performance]`, `[Avg Potential]`, `[Promotion Rate]`
* **Strategic Indicators:** `[High Performers %]`, `[Average Compa-Ratio]`, `[Turnover Rate]`

### Core Analytical Measures Highlight:

#### 1. Internal Compa-Ratio (Relational Row Iteration)
Iterates over each employee record, retrieving the respective pay grade midpoint from `Dim_JobRole` via `RELATED` to compute salary alignment against internal ranges:
```dax
Average Compa-Ratio = 
AVERAGEX(
    Fact_Performance,
    DIVIDE(
        Fact_Performance[AnnualSalary],
        RELATED(Dim_JobRole[SalaryMid]),
        1
    )
)

```

#### 2. High Performers % (Context Modification)

Calculates the proportion of top-tier talent by overriding the filter context on evaluation scores:

```dax
High Performers % = 
DIVIDE(
    CALCULATE(
        COUNTROWS(Fact_Performance),
        Fact_Performance[PerformanceRating] >= 4
    ),
    COUNTROWS(Fact_Performance),
    0
)

```

#### 3. Turnover Rate (Organizational Attrition)

Quantifies workforce attrition dynamically across organizational slices:

```dax
Turnover Rate = 
DIVIDE(
    SUM(Fact_Performance[LeftCompany]),
    [Total Headcount],
    0
)

```

---

## 🔍 Key Insights & Strategic Findings

1. **Compensation Alignment:** Identified departmental variations where average compa-ratio skewed below 96% despite sustained top-quartile performance ratings.
2. **Talent Distribution:** Mapped critical mass in the performance vs. potential matrix, isolating specific units requiring immediate succession planning.
3. **Workforce Demographics:** Highlighted age group concentrations (predominantly 20–39 age bands) to inform long-term retention and career-path progression policies.

---

## 🛠️ Tools & Technologies Used

* **Power BI Desktop:** Advanced data visualization, bookmarks, interactive matrix visuals, and card-based KPI layouts.
* **Power Query (M):** Data ingestion, schema standardization, data type optimization, and star schema dimensional staging.
* **DAX:** Dynamic aggregation, conditional filtering, iterative evaluation (`AVERAGEX`), and relational lookups (`RELATED`).

---

## 📬 Contact & Professional Profile

* **Author:** Gonçalo Matias Santos
* **Email:** goncalo.matias.data@gmail.com
