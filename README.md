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

## 📐 Key DAX Measures & Metrics Repository

All business logic is centralized in a dedicated `_Measures` table to maintain a clean semantic model and optimize calculation flow:

* **Core Demographics & Volume:** `[Total Headcount]`, `[Average Age]`
* **Talent & Evaluation:** `[Avg Performance]`, `[Avg Potential]`, `[High Performers %]`
* **Compensation & Mobility:** `[Avg Salary]`, `[Average Compa-Ratio]`, `[Promotion Rate]`, `[Turnover Rate]`

### Core Analytical Measures Highlight:

#### 1. High Performers %
Calculates the proportion of employees achieving top-tier performance ratings relative to total headcount:
```dax
High Performers % = 
DIVIDE(
    CALCULATE(COUNTROWS(Fact_Performance), Fact_Performance[PerformanceRating] >= 4),
    [Total Headcount],
    0
)

```

#### 2. Turnover Rate

Monitors attrition across segments to inform retention and succession strategies:

```dax
Turnover Rate = 
DIVIDE(
    CALCULATE(COUNTROWS(Fact_Performance), Fact_Performance[LeftCompany] = 1),
    [Total Headcount],
    0
)

```

#### 3. Average Compa-Ratio

Measures organizational pay positioning directly against predefined internal pay grade midpoints:

```dax
Average Compa-Ratio = 
AVERAGE(Fact_Performance[CompaRatio])

```

---

## 🔍 Key Insights & Strategic Findings

1. **Compensation Alignment:** Identified departmental variations where average compa-ratio skewed below 96% despite sustained top-quartile performance ratings.
2. **Talent Distribution:** Mapped critical mass in the 9-box performance vs. potential matrix, isolating specific units requiring immediate succession planning.
3. **Workforce Demographics:** Highlighted age group concentrations (predominantly 20–39 age bands) to inform long-term retention and career-path progression policies.

---

## 🛠️ Tools & Technologies Used

* **Power BI Desktop:** Advanced data visualization, bookmarks, interactive matrix visuals, and card-based KPI layouts.
* **Power Query (M):** Data ingestion, schema standardization, data type optimization, and star schema dimensional staging.
* **DAX:** Dynamic aggregation, conditional filtering, and custom KPI ratio formulas.

---

## 📬 Contact & Professional Profile

* **Author:** Gonçalo Matias Santos
* **Email:** goncalo.matias.data@gmail.com
