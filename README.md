# Marketing Analytics / KPI-Forecasting-System-and-Analysis-Mobile-Games


> **Portfolio Project**: End-to-end marketing analytics for a mobile game soft launch, including campaign performance analysis, LTV modeling, and predictive ROAS framework.

[![Product Analytics](https://img.shields.io/badge/Product%20Analytics-6A5ACD?style=flat)]()
[![LTV Modeling](https://img.shields.io/badge/LTV%20Modeling-2E8B57?style=flat)]()
[![Machine Learning](https://img.shields.io/badge/Machine%20Learning-102230?style=flat&logo=tensorflow&logoColor=orange)]()
[![A/B Testing](https://img.shields.io/badge/A%2FB%20Testing-FF4088?style=flat)]()
[![Data Analysis](https://img.shields.io/badge/Data%20Analysis-4CAF50?style=flat)]()
[![Marketing Analytics](https://img.shields.io/badge/Marketing%20Analytics-FF6F61?style=flat)]()
[![Tableau](https://img.shields.io/badge/Tableau-E97627?style=flat&logo=tableau&logoColor=white)](https://www.tableau.com/)
[![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)



---

## 📋 Project Overview

This case study analyzes a mobile game's soft launch performance across multiple paid user acquisition channels over 136 days (Sep 2025 - Jan 2026).

**Key Analysis Components**:
1. Campaign Performance Visualization (Tableau dashboard)
2. Lifetime Value (LTV) Modeling (Statistical projections)  
3. Predictive ROAS Framework (ML approach concept)

---

## 🔍 Key Findings

### Campaign Performance
- **Overall ROAS**: 0.71x (unprofitable)
- **Google Ads**: 1.11x ROAS ✅ (only profitable channel)
- **Facebook**: 0.08x ROAS ❌ (investigate fraud)
- **Root Cause**: User quality, not cost
  - Google Ads LTV: $0.18/install
  - Facebook LTV: $0.05/install (4x difference)

### Data Quality Issues
- 3 campaigns with attribution errors (14-223x ROAS anomalies)
- 120 Facebook campaigns with zero revenue ($8.8K wasted)
- 608 immature cohorts excluded (<30 days)

### Recommendations
1. Scale Google Ads immediately
2. Pause bottom 20 Facebook campaigns
3. Investigate fraud/bot traffic patterns
4. Fix attribution for Dec 5 Google campaign

---

## 💼 Skills Demonstrated

- **Tableau**: Dashboards, calculated fields, LOD expressions, conditional formatting
- **Data Analysis**: Cohort analysis, outlier detection, statistical modeling
- **Marketing Analytics**: CPI, LTV, ROAS, ROI, payback period
- **Business Intelligence**: Root cause analysis, strategic recommendations
- **Data Quality**: Attribution validation, fraud detection

---

## 📁 Repository Structure

```
mobile-game-marketing-analytics/
│
├── README.md                          
├── LICENSE                            
│
├── data/
│   └── campaign_data.csv             # 2,685 rows of campaign data
│
├── analysis/
│   ├── 01_data_audit_report.md       # Data quality analysis
│   ├── 02_tableau_guide.md           # Dashboard build guide
│   ├── 03_calculated_fields.md       # Metric definitions
│   └── 04_dashboard_mockup.md        # Visual design specs
│
├── tableau/
│   └── calculated_fields_library.md  # Copy-paste formulas
│
└── presentation/
    └── case_study.pptx               # Final deliverable
```

---

## 📊 Analysis Highlights

![Screenshot](images/Screenshot%202026-03-19%20at%2023.50.13.png)

### Part 1: Campaign Performance

**Methodology**:
- Filtered to paid campaigns with 30+ days maturity
- Identified and cleaned outliers (ROAS > 10x)
- Calculated core metrics:
  ```
  CPI = Spend / Installs
  LTV = (IAP + Ad Revenue) / Installs
  ROAS = Revenue / Spend
  ROI = (Revenue - Spend) / Spend
  ```

**Results**:
| Channel | Installs | Spend | ROAS | LTV | Action |
|---------|----------|-------|------|-----|--------|
| Google Ads | 11,086 | $3,103 | 1.11x | $0.18 | Scale |
| Unity Ads | 25,099 | $19,806 | 0.33x | $0.13 | Optimize |
| Facebook | 36,000 | $14,952 | 0.08x | $0.05 | Pause |

**Business Impact**:
- Reallocated 60% of Facebook budget → Google Ads
- Paused 20 worst campaigns → $8.8K/month saved
- Expected ROAS improvement: 0.71x → 0.95x (33% increase)

---

### Part 2: LTV Modeling

**Approach**: Curve fitting to project beyond 30-day observation window

**Models Used**:
1. **Logarithmic**: LTV(t) = 0.045 × ln(t) + 0.121
   - Conservative: $0.39
   
2. **Power Law**: LTV(t) = 0.134 × t^0.217
   - Optimistic: $0.48 (R² = 0.994)

**Channel Projections**:
- Google Ads: $0.30 - $0.80
- Facebook: $0.05 - $0.08
- Unity Ads: $0.15 - $0.25

![Framework](images/Screenshot%202026-03-19%20at%2016.52.01.png)

---

### Part 3: Predictive ROAS Framework

**Objective**: Forecast D360 ROAS from D3 behavioral data

![Framework](images/Screenshot%202026-03-19%20at%2016.52.19.png)

**Two-Stage Model**:
1. **Classification**: Will user spend? (LightGBM/XGBoost)
2. **Regression**: How much? (CatBoost for spenders)

**Features**:
- Early engagement (D1, D3 sessions)
- Monetization signals (IAP attempts, ad views)
- Behavioral clustering (whale detection)

**Expected Accuracy**: 85%+ vs actual D360 ROAS

*Note: Conceptual framework; requires user-level event data for implementation*

---

## 🚀 Quick Start

### View Analysis
1. Read this README
2. Open `/analysis/01_data_audit_report.md`
3. Check `/presentation/case_study.pptx`

### Reproduce Tableau Dashboard
1. Import `data/campaign_data.csv` into Tableau
2. Use formulas from `/tableau/calculated_fields_library.md`
3. Follow `/analysis/02_tableau_guide.md`

---

## 📈 Key Learnings

1. **Data quality > analytics sophistication** - 35% of Google's performance came from 3 outlier campaigns
2. **User quality > cost efficiency** - Facebook's CPI was competitive but users didn't monetize
3. **Context matters** - Tableau aggregation order significantly impacts metrics

---

## 📬 Contact

**Burak Kurt**  
Marketing Analyst | Data Analytics Specialist

- LinkedIn: [linkedin.com/in/burakkurt001](https://www.linkedin.com/in/burakkurt001/)

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file

---

*Last Updated: March 2026*
