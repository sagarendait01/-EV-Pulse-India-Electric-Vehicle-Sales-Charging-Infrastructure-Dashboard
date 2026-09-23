# ⚡ EV Pulse India — Electric Vehicle Sales & Charging Infrastructure Dashboard

An interactive **Power BI dashboard** analyzing India's Electric Vehicle (EV) market — state-wise adoption, manufacturer performance, and public charging infrastructure — built on a proper star-schema data model with DAX time intelligence.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-217346?style=for-the-badge)
![Power Query](https://img.shields.io/badge/Power%20Query-M-blue?style=for-the-badge)

---

## 📌 Overview

**EV Pulse India** turns raw EV sales and charging-station data into a single-page, interactive report that answers questions like:

- Which states are leading India's EV adoption?
- Which manufacturers dominate the market, and how has that changed over time?
- Is charging infrastructure keeping pace with EV sales growth?
- What's the year-over-year growth trend, and how does it vary by fiscal year/quarter?

---

## 🖼️ Dashboard Preview

> *(Add a screenshot of the report here — e.g. `docs/dashboard-preview.png`)*

```
![Dashboard Preview](docs/dashboard-preview.png)
```

---

## 🧩 Key Features

| Feature | Description |
|---|---|
| 📊 KPI Cards | Total EV Sales, YoY Growth %, EV Market Share %, Top State, Top Maker |
| 🗺️ Filled Map | Choropleth of EV sales across Indian states |
| 📈 Combo Chart | EV Sales trend + YoY Growth % by fiscal year |
| 🍩 Donut Chart | EV sales share by state |
| 🌳 Treemap | EV sales share by manufacturer |
| 🎀 Ribbon Chart | Manufacturer market-share shift across fiscal year/quarter |
| 📶 Bar Chart | Total vehicles sold by state |
| 🎚️ Slicers | Filter by vehicle category and state |

---

## 🗂️ Data Sources

| File | Description |
|---|---|
| `EV Sales By State.xlsx` | Date, state, vehicle category, EVs sold, total vehicles sold |
| `EV Sales By Makers.xlsx` | Date, vehicle category, manufacturer, EVs sold |
| `Charging Units.xlsx` | State-wise count of operational public charging stations (PCS) |
| `Dates.xlsx` | Date, fiscal year, quarter (custom fiscal calendar) |

> Data is transformed and loaded entirely via **Power Query (M)** — no manual pre-processing in Excel.

---

## 🏗️ Data Model (Star Schema)

```
        dim_states ──┐
                      ├──< FACT STATES >── Charging Units
   dim_vehicle_cat ───┘         |
                                 dim_Dates
        dim_Makers ──┐
                      ├──< FACT MAKERS >
   dim_vehicle_cat ───┘         |
                                 dim_Dates
```

- **Fact tables:** `FACT STATES`, `FACT MAKERS` — granular sales records linked to dimensions via surrogate keys
- **Dimension tables:** `dim_states`, `dim_Makers`, `dim_vehicle_category`, `dim_Dates`
- **Bridge table:** `Charging Units` joined to `dim_states` on `State Index`, enabling combined "sales vs. infrastructure" analysis
- Surrogate keys generated in Power Query with `Table.AddIndexColumn`, joined back via `Table.NestedJoin` + `Table.ExpandTableColumn`

---

## 🧮 DAX Measures

**Core aggregates**
- `Total EV Sales (States)` / `Total EV Sales (Makers)`
- `Total Vehicles Sold`
- `Total Charging Stations`

**Derived KPIs**
- `EV Market Share %` = EV Sales ÷ Total Vehicles Sold
- `Charging Stations per 1000 EVs`

**Time intelligence**
- `Sales Previous Year` (`DATEADD`, -1 Year)
- `YoY Growth %`
- `Sales QTD` (`TOTALQTD`)
- `Sales YTD` (`TOTALYTD`)

**Ranking & dynamic labels**
- `State Rank by Sales` / `Maker Rank by Sales` (`RANKX`, dense)
- `Top State` / `Top Maker` (`TOPN` + `SELECTEDVALUE`)
- `Selected Period` — dynamically shows "All Periods" or "FY <year>" based on active slicer

---

## 🛠️ Tech Stack

- **Power BI Desktop** — report authoring & visualization
- **Power Query (M)** — data extraction, cleaning, and star-schema construction
- **DAX** — measures, time intelligence, ranking logic
- **Excel** — source data format

---

## 🚀 Getting Started

1. Clone this repository
   ```bash
   git clone https://github.com/<your-username>/ev-pulse-india.git
   ```
2. Open `CA2_MAIN1.pbit` in **Power BI Desktop**
3. When prompted, point the four data source files (`EV Sales By State.xlsx`, `EV Sales By Makers.xlsx`, `Charging Units.xlsx`, `Dates.xlsx`) to their location on your machine
4. Let Power BI refresh the data model — the report will render automatically

> 💡 This is a `.pbit` **template** file — it ships without data baked in, so it will ask for the source files on first open.

---

## 📈 Future Enhancements

- [ ] Add a second page with maker-level drill-through
- [ ] Incorporate population-normalized EV adoption rate per state
- [ ] Automate data refresh via a scheduled pipeline
- [ ] Add forecast visuals for next fiscal year sales

---

## 📄 License

This project is for educational/portfolio purposes. Attribute the dataset source if reused.
