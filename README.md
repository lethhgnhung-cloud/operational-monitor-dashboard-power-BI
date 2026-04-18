# 💰 Operational Expenditure Intelligence Dashboard

> **An interactive Power BI dashboard for real-time OPEX monitoring — tracking actual spend vs. budget across branches, categories, and time periods to enable faster, data-driven financial decisions.**

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Advanced-blue?style=for-the-badge)
![Power Query](https://img.shields.io/badge/Power%20Query-M--Language-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Problem Statement](#-problem-statement)
- [Data Architecture & Modeling](#-data-architecture--modeling)
- [KPIs & Analytical Metrics](#-kpis--analytical-metrics)
- [Advanced DAX Implementation](#-advanced-dax-implementation)
- [Dashboard Features](#-dashboard-features)
- [Business Insights & Actions](#-business-insights--actions)
- [Impact & Deliverables](#-impact--deliverables)
- [Screenshot](#-screenshot)
- [How to Use](#-how-to-use)

---

## 🎯 Project Overview

This project delivers a production-ready **Operational Expenditure (OPEX) Intelligence Dashboard** built in Power BI. Designed for multi-branch organizations, it replaces fragmented monthly Excel reports with an automated, self-service analytics platform — giving finance teams and branch managers a live, interactive view of their financial health.

| Attribute | Details |
|-----------|---------|
| **Tool** | Microsoft Power BI Desktop |
| **Data Model** | Star Schema |
| **Transformation Layer** | Power Query (M Language) |
| **Analytics Layer** | Advanced DAX — Variance, YTD, Time Intelligence |
| **Primary KPI** | Actual Expenditure vs. Budget |
| **Stakeholders** | C-Suite · Finance · Branch Managers · Department Heads |

---

## 🔍 Problem Statement

Prior to this solution, the organization managed OPEX through a manually compiled monthly reporting process, creating three significant operational gaps:

| Pain Point | Business Impact |
|------------|----------------|
| **Fragmented reporting** | Expense data was scattered across local branch reports with no consolidated view |
| **Delayed overspend detection** | Budget overruns were only discovered at month-end, leaving no time for corrective action |
| **No benchmarking capability** | Branch-level performance comparisons were nearly impossible due to inconsistent data formats |

This dashboard directly resolves all three gaps by automating consolidation and delivering a unified, real-time financial lens.

---

## 🏗️ Data Architecture & Modeling

### Star Schema Design

The data model follows a **Star Schema** pattern, optimized for Power BI's Vertipaq engine to ensure fast query performance and clean cross-filtering.

```
                    ┌──────────────┐
                    │  dim_Date    │
                    └──────┬───────┘
                           │
┌──────────────┐    ┌──────▼────────┐    ┌──────────────────┐
│  dim_Branch  ├────►  fct_Expense  │◄───┤ dim_ExpCategory  │
└──────────────┘    └───────────────┘    └──────────────────┘
```

| Table | Type | Description |
|-------|------|-------------|
| `fct_Expense` | Fact | Centralized transactional spending records |
| `dim_Branch` | Dimension | Branch / department metadata |
| `dim_ExpCategory` | Dimension | Expense category classification |
| `dim_Date` | Dimension | Full calendar table for time-intelligence functions |

### Data Preparation — Power Query (M Language)

Raw source data was cleaned and transformed through an automated Power Query pipeline covering:

- **Standardization** — Unified inconsistent branch naming conventions and category codes across source files
- **Data Type Enforcement** — Ensured correct types for all date, currency, and numeric fields
- **Null & Error Handling** — Replaced missing budget entries with `0` to prevent DAX measure breakage
- **Calendar Table Generation** — Built a complete `dim_Date` table dynamically from the data's min/max date range to support time-intelligence functions

---

## 📊 KPIs & Analytical Metrics

The dashboard is anchored on five core financial health indicators, organized from strategic to operational:

| # | KPI | Business Question Answered |
|---|-----|---------------------------|
| 1 | **Actual Expenditure** | How much have we spent in total? |
| 2 | **Budget Allocation** | What is our approved spending limit? |
| 3 | **Budget Variance (%)** | Are we over or under budget, and by how much? |
| 4 | **Remaining Budget** | How much budget is still available? |
| 5 | **% Budget Used** | What proportion of our budget has been consumed? |

All KPIs are fully responsive to slicer selections — filtering by branch, category, or time period updates every metric simultaneously.

---

## 🧮 Advanced DAX Implementation

### Core Financial Measures

**Budget Variance %**
Calculates the percentage deviation between actual spend and budget. Positive values signal overspend; negative values indicate underspend.

```dax
Budget Variance % =
DIVIDE(
    [Actual Expenditure] - [Budget],
    [Budget],
    0
)
```

**Remaining Budget**
Returns the outstanding budget available for the selected scope and period.

```dax
Remaining Budget =
[Budget] - [Actual Expenditure]
```

**% Budget Used**
Measures the proportion of the total budget that has been consumed — used to drive conditional color-coding on gauge and KPI visuals.

```dax
% Budget Used =
DIVIDE(
    [Actual Expenditure],
    [Budget],
    0
)
```

**YTD Actual Expenditure**
Accumulates spend from the start of the fiscal year to the currently selected date context.

```dax
YTD Actual Expenditure =
TOTALYTD(
    [Actual Expenditure],
    dim_Date[Date]
)
```

**Monthly Trend (Running Total)**
Supports trend line visuals by computing a month-over-month cumulative spend.

```dax
MTD Expenditure =
TOTALMTD(
    [Actual Expenditure],
    dim_Date[Date]
)
```

> All measures are designed to return `BLANK()` or `0` gracefully when no budget data exists, preventing misleading visuals on out-of-scope filters.

---

## 🖥️ Dashboard Features

### Dynamic Filtering
Three interconnected slicers allow users to slice the entire dashboard from any angle without navigating away from the page:

- **Expense Category** — Isolate a specific cost driver (e.g., Rent, Utilities, Salaries)
- **Branch** — Compare or audit individual locations
- **Year** — Perform period-over-period comparisons

### Time-Intelligence Toggles
Seamlessly switch between **Quarterly** and **Monthly** aggregation views using bookmark-driven navigation buttons — no page reload required.

### Interactive Drill-Downs
All chart visuals support **click-to-filter** cross-highlighting. Selecting a branch in the bar chart simultaneously filters the trend line, KPI cards, and category breakdown — enabling a complete top-down to granular investigation within a single dashboard page.

### Conditional Formatting (DAX-Driven)
Visual alerts are embedded directly into KPI cards and tables using DAX-based formatting rules:

| Condition | Visual Cue |
|-----------|-----------|
| `% Budget Used` > 100% | 🔴 Red — Budget overrun |
| `% Budget Used` between 80%–100% | 🟡 Yellow — Approaching limit |
| `% Budget Used` < 80% | 🟢 Green — On track |

---

## 💡 Business Insights & Actions

Data exploration surfaced three strategic patterns that directly influenced management decisions:

### 1 — Rent Dominates Total OPEX (68%)
Rent was identified as the largest single cost driver at **~15.67 billion VND, representing approximately 68% of total expenditure**.

> **Action:** Triggered an immediate lease contract review across all branches. Findings were escalated to senior management to negotiate renegotiation terms with landlords for the upcoming renewal cycle.

---

### 2 — Head Office Accountability Gap
The Head Office alone accounted for **14.5 billion VND in spending** — a concentration that had never been previously visible in the fragmented reporting system.

> **Action:** A dedicated HO cost audit was commissioned, reviewing overhead allocation, vendor contracts, and departmental budgets against approved limits.

---

### 3 — Recurring January Spend Peak
A consistent surge in expenditure was identified every **January** across multiple years — driven by front-loaded annual contracts, renewals, and Q1 operational ramp-up costs.

> **Action:** Budget planning methodology was updated to **front-load Q1 allocations** in subsequent fiscal years, preventing the recurring mid-quarter shortfall and improving cash flow forecasting accuracy.

---

## 🚀 Impact & Deliverables

| Dimension | Outcome |
|-----------|---------|
| **Operational Efficiency** | Automated the full reporting pipeline — eliminated manual monthly data consolidation |
| **Data Accuracy** | Removed human-entry error risk through a standardized, automated ETL workflow in Power Query |
| **Financial Accountability** | Gave branch managers real-time visibility into their own remaining budget, fostering proactive spend management |
| **Decision Speed** | Shifted finance reviews from monthly retrospective reports to real-time, on-demand analysis |

---

## 📸 Screenshot

| View | Preview |
|------|---------|
| Full Dashboard | [View Snapshot](https://github.com/lethhgnhung-cloud/operational-monitor-dashboard-power-BI/blob/main/snapshot%20of%20dashboard.png) |

---

## 🚀 How to Use

**Prerequisite:** [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free, latest version recommended)

1. **Clone or download this repository**
   ```bash
   git clone https://github.com/lethhgnhung-cloud/operational-monitor-dashboard-power-BI.git
   ```

2. **Open the report file**
   - Launch Power BI Desktop
   - Open the `.pbix` file from the repository root

3. **Explore the data model**
   - Go to **Model View** to inspect the Star Schema, relationships, and cardinality
   - Review all DAX measures in the **Data Pane** under each table

4. **Interact with the dashboard**
   - Use the **slicers** (Branch, Category, Year) to filter the full report
   - Click any chart element to **cross-filter** all other visuals
   - Toggle between **Quarterly / Monthly** views using the navigation buttons

--

*Found this project useful? Feel free to ⭐ star the repository.*
