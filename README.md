<div align="center">

# 🏆 Nexoria Group — Executive KPI Report
### ZoomCharts 4U Report Challenge · March Edition

[![Challenge](https://img.shields.io/badge/Challenge-ZoomCharts%204U%20Report-1A73E8?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0xMiAyQzYuNDggMiAyIDYuNDggMiAxMnM0LjQ4IDEwIDEwIDEwIDEwLTQuNDggMTAtMTBTMTcuNTIgMiAxMiAyeiIvPjwvc3ZnPg==)](https://zoomcharts.com)
[![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com)
[![DAX](https://img.shields.io/badge/DAX-108%20Measures-0F4C8A?style=for-the-badge)](https://learn.microsoft.com/en-us/dax/)
[![IBCS](https://img.shields.io/badge/IBCS-Standards-34A853?style=for-the-badge)](https://www.ibcs.com)
[![Status](https://img.shields.io/badge/Status-In%20Progress-F9AB00?style=for-the-badge)]()

<br/>

> **Building a board-ready Executive KPI Report for Nexoria Group** — a fictional global enterprise operating across Technology, Operations and Commercial divisions in AMER, EMEA and APAC.

<br/>

[📊 View Live Report](#) · [📄 Read the Docs](./docs/) · [💬 Challenge Page](https://zoomcharts.com/en/microsoft-power-bi-custom-visuals/challenges/)

</div>

---

## 📌 Table of Contents

- [Challenge Brief](#-challenge-brief)
- [Report Overview](#-report-overview)
- [Dashboard Architecture](#-dashboard-architecture)
- [Data Model](#-data-model)
- [DAX Measures](#-dax-measures)
- [Technologies Used](#-technologies-used)
- [Repository Structure](#-repository-structure)
- [Getting Started](#-getting-started)
- [Design Decisions](#-design-decisions)
- [Screenshots](#-screenshots)
- [Roadmap](#-roadmap)
- [License](#-license)

---

## 🎯 Challenge Brief

The **ZoomCharts 4U Report Challenge** requires building a board-ready executive report that follows the **4i principles** — Inspiring, Intuitive, Interactive, and Insightful.

| Requirement | Details |
|---|---|
| **Dataset** | 440,000+ rows · 10 KPIs · 24 months |
| **Scenarios** | Actual · Budget · Forecast |
| **Regions** | AMER · EMEA · APAC |
| **Divisions** | Technology · Operations · Commercial |
| **ZoomCharts** | Minimum 2 Drill Down PRO visuals on the same page |
| **Pages** | 3–4 maximum including drill-through |

---

## 📊 Report Overview

The report answers four board-level questions — one per page:

| Page | Question | 4i Principle |
|---|---|---|
| **P1 · Executive Summary** | Is the business on track? | Inspiring + Intuitive |
| **P2 · Financial Deep-Dive** | Where are the financial drivers? | Interactive + Insightful |
| **P3 · Regional & Ops** | How are regions and operations performing? | Interactive + Insightful |
| **P4 · KPI Detail** | Why is this KPI where it is? *(drill-through)* | Insightful |

### KPIs Covered

| Category | KPI | Polarity | Target |
|---|---|---|---|
| Financial | Revenue | HigherIsBetter | Budget |
| Financial | EBITDA | HigherIsBetter | Budget |
| Financial | Gross Margin % | HigherIsBetter | Budget |
| People | Headcount | HigherIsBetter | Budget |
| Customer | Net Customer Adds | HigherIsBetter | **YoY** |
| Customer | Active Customers | HigherIsBetter | **YoY** |
| Operations | On-time Delivery % | HigherIsBetter | Budget |
| Operations | System Uptime % | HigherIsBetter | Budget |
| Operations | Support Tickets | **LowerIsBetter** | Budget |
| Operations | Cycle Time (days) | **LowerIsBetter** | Budget |

---

## 🏗️ Dashboard Architecture

```
┌─────────────────────────────────────────────────────────────┐
│  GLOBAL SLICERS  Year · Month · Scenario · Division · Region │
├─────────────────────────────────────────────────────────────┤
│  P1 — Executive Summary          [Inspiring + Intuitive]    │
│  ┌─────────────┐  ┌──────────────────────────────────────┐  │
│  │  5 KPI Cards │  │  ZoomCharts Bar Drill Down PRO       │  │
│  │  ▲ Revenue  │  │  Revenue by Division → BU → Product  │  │
│  │  ▲ EBITDA   │  ├──────────────────────────────────────┤  │
│  │  ● Margin   │  │  ZoomCharts Line Drill Down PRO       │  │
│  │  ▼ HC       │  │  24-Month Revenue Actual vs Budget   │  │
│  │  ▲ Customers│  └──────────────────────────────────────┘  │
│  └─────────────┘  ┌──────────────────────────────────────┐  │
│                   │  IBCS Matrix — All 10 KPIs Scorecard  │  │
│                   └──────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────┤
│  P2 — Financial Deep-Dive        [Interactive + Insightful] │
│  P3 — Regional & Ops             [Interactive + Insightful] │
│  P4 — KPI Detail (drill-through) [Insightful]               │
└─────────────────────────────────────────────────────────────┘
```

---

## 🗃️ Data Model

```
FactKPI_Monthly (440,000+ rows)
    │── MonthKey ──────────► DimDate
    │── OrgKey ────────────► DimOrg
    │── RegionKey ─────────► DimRegion
    │── KPIKey ────────────► DimKPI
    └── Scenario, Value
```

| Table | Key Fields | Purpose |
|---|---|---|
| `FactKPI_Monthly` | MonthKey, OrgKey, RegionKey, Scenario, KPIKey, Value | Core fact table |
| `DimDate` | MonthKey, Year, Quarter, MonthName, **IsBoardSeason** | Time dimension |
| `DimOrg` | Division, BusinessUnit, ProductLine | Org hierarchy |
| `DimRegion` | Region, Subregion, Country, Latitude, Longitude | Geography |
| `DimKPI` | KPIName, KPICategory, Polarity, FormatString, DefaultTargetType | KPI metadata |

---

## 📐 DAX Measures

**108 measures** organized in 7 display folders:

```
00 - Core Scenarios         → Actual Value, Budget Value, Forecast Value
01 - Variances              → Absolute and % variances vs Budget and Forecast
02 - Financial KPIs         → Revenue, EBITDA, Gross Margin (Actual/Budget/Forecast)
03 - People KPIs            → Headcount (Actual/Budget/Forecast/Variance)
04 - Customer KPIs          → Active Customers, Net Customer Adds (Actual/PY/YoY%)
05 - Operations KPIs        → Delivery, Uptime, Support Tickets, Cycle Time
05 - Time Intelligence      → YTD, PY, YoY Growth %
06 - UX Helpers             → KPI Status Color, KPI Status Icon (polarity-aware)
07 - IBCS Visuals           → 24 SVG ImageUrl measures (PowerofBI.IBCS UDFs)
```

### Key DAX Pattern — Polarity-aware KPI Status

```dax
KPI Status Color =
VAR _polarity = SELECTEDVALUE(DimKPI[Polarity], "HigherIsBetter")
VAR _var = [Actual vs Budget (%)]
RETURN
    IF(
        ISBLANK(_var), "#9E9E9E",
        IF(
            _polarity = "HigherIsBetter",
            IF(_var >= 0.02, "#34A853", IF(_var >= -0.02, "#F9AB00", "#EA4335")),
            IF(_var <= -0.02, "#34A853", IF(_var <= 0.02, "#F9AB00", "#EA4335"))
        )
    )
```

---

## ⚙️ Technologies Used

| Technology | Role |
|---|---|
| **Power BI Desktop** | Report development environment |
| **ZoomCharts Drill Down Bar PRO** | Org hierarchy drill-down (Division → BU → Product) |
| **ZoomCharts Drill Down Line PRO** | 24-month time-series with Year → Quarter → Month drill |
| **PowerofBI.IBCS UDF Library** | 24 IBCS-standard SVG chart measures (ImageUrl) |
| **DAX** | 108 measures across 7 folders |
| **Power BI Modeling MCP** | AI-assisted batch DAX measure engineering via Claude |
| **Custom JSON Theme** | Nexoria Executive Theme (brand colors + Segoe UI) |

---

## 📁 Repository Structure

```
nexoria-executive-kpi-report/
│
├── 📄 README.md                          # This file
├── 📄 CHANGELOG.md                       # Version history
├── 📄 .gitignore                         # Git ignore rules
│
├── 📂 .github/
│   └── 📂 ISSUE_TEMPLATE/
│       ├── bug_report.md                 # Bug report template
│       └── feature_request.md            # Feature request template
│
├── 📂 assets/
│   ├── 📂 screenshots/
│   │   ├── p1-executive-summary.png      # Page 1 screenshot
│   │   ├── p2-financial-deepdive.png     # Page 2 screenshot
│   │   ├── p3-regional-ops.png           # Page 3 screenshot
│   │   └── p4-kpi-detail.png             # Page 4 screenshot
│   └── 📂 theme/
│       └── Nexoria_Executive_Theme.json  # Power BI theme file
│
├── 📂 dax/
│   ├── 📂 00-core-scenarios/
│   │   └── core-scenarios.dax            # Actual, Budget, Forecast
│   ├── 📂 01-variances/
│   │   └── variances.dax                 # Variance vs Budget and Forecast
│   ├── 📂 02-financial/
│   │   └── financial-kpis.dax            # Revenue, EBITDA, Gross Margin
│   ├── 📂 03-people/
│   │   └── people-kpis.dax               # Headcount measures
│   ├── 📂 04-customer/
│   │   └── customer-kpis.dax             # Active Customers, Net Adds
│   ├── 📂 05-operations/
│   │   └── operations-kpis.dax           # Delivery, Uptime, Tickets, Cycle Time
│   ├── 📂 05-time-intelligence/
│   │   └── time-intelligence.dax         # YTD, PY, YoY Growth
│   ├── 📂 06-ux-helpers/
│   │   └── ux-helpers.dax                # Status Color, Status Icon, Selected KPI
│   └── 📂 07-ibcs-visuals/
│       ├── ibcs-financial.dax            # Revenue, EBITDA, Gross Margin IBCS
│       ├── ibcs-people.dax               # Headcount IBCS
│       ├── ibcs-customer.dax             # Customer IBCS
│       └── ibcs-operations.dax           # Ops IBCS (LowerIsBetter variants)
│
├── 📂 docs/
│   ├── dashboard-structure.md            # Full visual-by-visual specification
│   ├── data-model.md                     # Model documentation
│   ├── kpi-definitions.md                # KPI logic and DAX references
│   ├── ibcs-conventions.md               # IBCS coding rules for this model
│   └── design-decisions.md               # Why I made these choices
│
└── 📂 dataset/
    └── README.md                         # Dataset source and download instructions
```

---

## 🚀 Getting Started

### Prerequisites

- Power BI Desktop (latest version)
- ZoomCharts Drill Down PRO license ([free developer license available](https://zoomcharts.com))
- PowerofBI.IBCS UDF package ([powerofbi.org/ibcs](https://powerofbi.org/ibcs))

### Setup

```bash
# 1. Clone this repository
git clone https://github.com/YOUR_USERNAME/nexoria-executive-kpi-report.git

# 2. Open the .pbix file in Power BI Desktop
# File → Open → Executive_KPI_Report.pbix

# 3. Apply the theme
# View → Themes → Browse for themes → assets/theme/Nexoria_Executive_Theme.json

# 4. Connect your ZoomCharts license
# File → Options → Preview features → ZoomCharts License Key
```

---

## 💡 Design Decisions

### 1. Pages named by question, not by data
Instead of "Revenue Page", the page is called **"Where Are the Financial Drivers?"** — this sets the cognitive frame before the user sees any visual.

### 2. Zero filter panels
Every interaction is a click on a ZoomCharts visual. No filter pane is required. Cross-filtering propagates automatically across all visuals on the page.

### 3. Polarity-aware status logic
`KPI Status Color` reads `DimKPI[Polarity]` and **automatically reverses** the green/red logic for `LowerIsBetter` KPIs (Support Tickets, Cycle Time). No hard-coded exceptions.

### 4. IsBoardSeason reference band
The `DimDate[IsBoardSeason]` flag (March–April) adds a subtle reference band to all trend charts. Board-season months are highlighted without any text annotation.

### 5. IBCS businessImpact = -1 for operational KPIs
`IBCS | Support Tickets – Bar Absolute Variance` uses `businessImpact = -1` — meaning a *negative* variance vs Budget is rendered in green. Fewer tickets than planned is a good outcome.

---

## 📸 Screenshots

> Screenshots will be added once the report is finalized and submitted.

| Page | Preview |
|---|---|
| P1 — Executive Summary | `assets/screenshots/p1-executive-summary.png` |
| P2 — Financial Deep-Dive | `assets/screenshots/p2-financial-deepdive.png` |
| P3 — Regional & Ops | `assets/screenshots/p3-regional-ops.png` |
| P4 — KPI Detail | `assets/screenshots/p4-kpi-detail.png` |

---

## 🗺️ Roadmap

- [x] Data model exploration and measure inventory
- [x] 108 DAX measures created (7 display folders)
- [x] 24 IBCS SVG measures (PowerofBI.IBCS UDFs)
- [x] Custom Power BI JSON theme
- [x] Full dashboard architecture specification
- [ ] Report build — Page 1 (Executive Summary)
- [ ] Report build — Page 2 (Financial Deep-Dive)
- [ ] Report build — Page 3 (Regional & Ops)
- [ ] Report build — Page 4 (KPI Detail drill-through)
- [ ] Screenshot documentation
- [ ] Challenge submission
- [ ] Live report link (post-validation)

---

## 📜 License

This project is submitted as part of the [ZoomCharts 4U Report Challenge](https://zoomcharts.com/en/microsoft-power-bi-custom-visuals/challenges/).

The dataset is provided by ZoomCharts for challenge participants only and is not included in this repository. See [`dataset/README.md`](./dataset/README.md) for download instructions.

---

<div align="center">

Made with ☕ and DAX · Powered by [ZoomCharts](https://zoomcharts.com) · Built to IBCS standards

**If this project helped you, consider giving it a ⭐**

</div>
