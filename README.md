# Pharmaceutical Inventory & Demand Planning Analytics

**An end-to-end analytics and planning project combining ABC-XYZ classification, safety stock optimization, and Holt-Winters demand forecasting across 12 pharmaceutical drug categories.**

---

## Project Background

This project applies supply chain analytics to a real pharmaceutical sales dataset to answer three operational questions:

1. **Which drugs deserve the most inventory management attention?** → ABC-XYZ Classification
2. **How much safety stock should we hold and when should we reorder?** → Safety Stock & ROP Modeling
3. **What demand should we plan for in 2020?** → Holt-Winters Demand Forecasting

---

## Dashboard Preview

> Built in Power BI | 4 interactive pages

| Page | Description |
|------|-------------|
| Overview | Intro page with key KPIs and navigation |
| Drug Portfolio Overview | ABC-XYZ classification matrix and sales distribution |
| Reorder Alerts & Forecast Accuracy | Safety stock status, reorder alerts, and MAPE summary |
| Demand Forecast | Actual vs forecasted demand with 2020 plan and confidence intervals |

---

## Dataset

**Source:** Pharmaceutical Sales Dataset (public)  
**Period:** January 2014 — October 2019  
**Granularity:** Monthly sales by drug category  
**Drug categories:** 12 (ATC classification system)

| ATC Code | Drug Category | ABC | XYZ |
|----------|--------------|-----|-----|
| N02BE | Paracetamol | A | X |
| N05B | Anxiolytics | A | X |
| A11CC05 | Vitamin D | A | Y |
| R03 | Inhalers | A | X |
| M01AB | Diclofenac | B | X |
| A07CA | ORS | B | Y |
| M01AE | Ibuprofen | B | X |
| N02BA | Aspirin | C | X |
| R06 | Antihistamines | C | Y |
| P01BA01 | Antimalarial | C | Z |
| J05AH02 | Oseltamivir | C | Z |
| N05C | Hypnotics | C | X |

> 4 additional drugs (Oseltamivir, ORS, Vitamin D, Antimalarial) were generated with realistic seasonal patterns based on Saudi Arabia healthcare context — Hajj season, summer heat, winter vitamin D deficiency.

---

## Tools & Methods

| Tool | Purpose |
|------|---------|
| Python (Pandas, Statsmodels) | Data cleaning, outlier treatment, forecasting |
| Excel | ABC-XYZ classification, Safety Stock & ROP calculations |
| Power BI (PL-300 Certified) | Interactive dashboard and visualization |
| GitHub | Version control and portfolio sharing |

---

## Key Methods

### ABC-XYZ Classification
- **ABC** — classifies drugs by revenue contribution using Pareto analysis (A = top 70%, B = next 20%, C = remaining 10%)
- **XYZ** — classifies drugs by demand variability using Coefficient of Variation (X < 0.5, Y = 0.5–1.0, Z > 1.0)
- Combined classification drives differentiated inventory strategies per SKU

### Safety Stock & Reorder Point
- Safety Stock = Z × σ(daily demand) × √(Lead Time)
- Service levels linked to ABC class: A = 99%, B = 95%, C = 90%
- Z-scores calculated using NORM.S.INV() function
- Lead times differentiated by drug type (7 days standard, 14 days controlled substances, 21 days specialist procurement)

### Demand Forecasting
- **Method:** Holt-Winters Exponential Smoothing (additive trend + additive seasonality)
- **Train period:** Jan 2014 — Dec 2018 (60 months)
- **Test period:** Jan 2019 — Oct 2019 (10 months) — used to calculate MAPE
- **Forecast period:** Jan 2020 — Dec 2020 (12 months) with 95% confidence intervals
- **Outlier treatment:** Winsorization applied to October 2019 anomaly (70-85% drop across all categories identified as data quality issue)

### Forecast Accuracy (MAPE Results)

| Drug | MAPE | Status |
|------|------|--------|
| ORS | 2.4% | ✅ Excellent |
| Vitamin D | 7.1% | ✅ Excellent |
| Diclofenac | 10.1% | ✅ Good |
| Paracetamol | 12.4% | ✅ Good |
| Ibuprofen | 15.0% | ✅ Good |
| Antihistamines | 20.3% | ⚠️ Acceptable |
| Aspirin | 22.1% | ⚠️ Acceptable |
| Hypnotics | 21.2% | ⚠️ Acceptable |
| Anxiolytics | 30.0% | ⚠️ Acceptable |
| Inhalers | 50.4% | ❌ Manual Planning |
| Antimalarial | 154.0% | ❌ Manual Planning |
| Oseltamivir | 205.2% | ❌ Manual Planning |

> Inhalers, Antimalarial, and Oseltamivir are **event-driven** — their demand is governed by flu season and the Hajj Islamic calendar which shifts annually. Statistical models cannot capture these patterns without external event triggers. These drugs are flagged for manual planning overlay.

---

## Key Insights

**1 — Paracetamol is the most urgent reorder priority**
Highest daily demand (29.92 units/day) with current stock at 280 units against a ROP of 305 — only 9 days of supply remaining against a 7-day lead time. Immediate procurement action required.

**2 — Antimalarial demand is ungovernable by algorithm**
CV of 2.46 — the highest in the portfolio. Demand concentrates in a single month per year corresponding to Hajj season, and that month shifts every year with the Islamic calendar. This requires a manual calendar overlay on top of any statistical forecast.

**3 — Anxiolytics require separate procurement treatment**
Classified as A-class by volume (14% of total sales) but are controlled substances in Saudi Arabia — standard automated reorder triggers do not apply. Regulatory approval process must be built into the lead time.

**4 — October 2019 data quality issue**
A 70-85% drop across all drug categories in October 2019 was identified as a data quality anomaly — not a real demand event. Treated with Winsorization before modeling. Demonstrates the importance of data validation before forecasting.

**5 — Portfolio is 75% concentrated in 4 items**
The top 4 A-class drugs (Paracetamol, Anxiolytics, Vitamin D, Inhalers) account for approximately 75% of total sales volume — classic Pareto distribution confirmed.

---

## Repository Structure

```
pharma-inventory-demand-planning/
│
├── pharma_forecast.ipynb       ← Python notebook (forecasting)
├── forecast_results.csv        ← Python output: all forecasts
├── mape_summary.csv            ← Python output: accuracy summary
├── Main_worksheet.xlsx       ← Excel: ABC-XYZ + Safety Stock + ROP
│
├── dashboard_screenshots/
│   ├── overview.png
│   ├── portfolio_overview.png
│   ├── reorder_alerts.png
│   └── demand_forecast.png
│
└── README.md
```

---

## How to Run

1. Open `pharma_forecast.ipynb` in Google Colab
2. Upload `working_data.csv` when prompted
3. Run all cells — outputs are `forecast_results.csv` and `mape_summary.csv`
4. Import all CSV files and the Excel file into Power BI Desktop
5. Open the Power BI file to explore the interactive dashboard

---

## About the Author

**Hossam Badawy**  
Licensed Pharmacist | Supply Chain & Inventory Analyst  
Power BI Microsoft Certified (PL-300) | SQL | Python | Excel  

📎 [LinkedIn](https://linkedin.com/in/hossam-badawy)


