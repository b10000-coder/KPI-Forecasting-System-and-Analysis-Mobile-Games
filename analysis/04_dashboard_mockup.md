# 📱 DASHBOARD MOCKUP - VISUAL GUIDE
## What Your Final Dashboard Should Look Like

---

# DASHBOARD 1: EXECUTIVE SUMMARY

```
╔════════════════════════════════════════════════════════════════════╗
║  MOBILE GAME COMPANY CAMPAIGN PERFORMANCE - EXECUTIVE SUMMARY                ║
║  Date Range: Sep 2025 - Jan 2026  |  View: Mature Cohorts (30+ days)║
╠════════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐         ║
║  │  $52.8K  │  │  72,785  │  │  $13.5K  │  │  0.71x   │         ║
║  │  Spend   │  │ Installs │  │ Revenue  │  │  ROAS    │         ║
║  │          │  │          │  │          │  │  🔴      │         ║
║  └──────────┘  └──────────┘  └──────────┘  └──────────┘         ║
║                                                                    ║
║  ┌──────────┐  ┌──────────┐                                      ║
║  │  $0.73   │  │   -29%   │                                      ║
║  │   CPI    │  │   ROI    │                                      ║
║  │          │  │   🔴     │                                      ║
║  └──────────┘  └──────────┘                                      ║
║                                                                    ║
╠════════════════════════════════════════════════════════════════════╣
║  ROAS BY CHANNEL (D30, Mature Campaigns)                          ║
║                                                                    ║
║  Google Ads      ████████████████ 1.11x  🟢                       ║
║  Unity Ads       ██ 0.33x  🔴                                     ║
║  Facebook        ▌ 0.08x  🔴                                      ║
║  ASO Keywords    ▌ 0.06x                                          ║
║                  │                                                 ║
║                 0.0      0.5      1.0      1.5      2.0           ║
║                          ↑ Break-even                             ║
║                                                                    ║
╠════════════════════════════════════════════════════════════════════╣
║  🎯 KEY INSIGHTS                                                   ║
║                                                                    ║
║  ⚠️  Game is currently unprofitable (0.71x ROAS, -29% ROI)       ║
║  ✅  Google Ads is the only profitable channel (1.11x ROAS)       ║
║  🔴  Facebook losing 92% - investigate for fraud/poor targeting   ║
║  💡  Recommendation: Scale Google Ads, pause Facebook campaigns   ║
║                                                                    ║
╠════════════════════════════════════════════════════════════════════╣
║  [Filters]  Channel: [All ▼]  Date: [───●───]  Min Installs: [50]║
╚════════════════════════════════════════════════════════════════════╝

Navigation: [ >> Channel Deep Dive ]  [ Data Quality >> ]
```

---

# DASHBOARD 2: CHANNEL PERFORMANCE

```
╔════════════════════════════════════════════════════════════════════╗
║  CHANNEL PERFORMANCE DEEP DIVE                                     ║
╠════════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  ┌─────────────────────────────┐  ┌─────────────────────────────┐║
║  │ CHANNEL EFFICIENCY MATRIX   │  │ DAILY TREND                 │║
║  │                             │  │                             │║
║  │  LTV D30 ($)                │  │ Installs                    │║
║  │   ↑                         │  │    ↑                        │║
║  │0.4│    ⭐ Google            │  │ 400│    ╱╲                  │║
║  │   │     ● (50+ campaigns)   │  │    │   ╱  ╲  ╱╲            │║
║  │0.3│                         │  │ 300│  ╱    ╲╱  ╲           │║
║  │   │                         │  │    │ ╱          ╲          │║
║  │0.2│                         │  │ 200│╱            ╲──       │║
║  │   │                         │  │    │                       │║
║  │0.1│      Facebook ●●●       │  │ 100│       Spend ($)       │║
║  │   │      (120 campaigns)    │  │    │    ╱──╲               │║
║  │ 0 ├────────────────────→    │  │  0 ├───────────────────→   │║
║  │   0   0.5   1.0   1.5  CPI  │  │    Sep  Oct  Nov  Dec  Jan │║
║  │                             │  │                             │║
║  │ ● Google  ● Facebook        │  │ ─ Installs  ─ Spend       │║
║  │ ● Unity   ● ASO             │  │                             │║
║  └─────────────────────────────┘  └─────────────────────────────┘║
║                                                                    ║
╠════════════════════════════════════════════════════════════════════╣
║  CAMPAIGN DETAIL TABLE (Sortable)                                 ║
║                                                                    ║
║  Channel      Campaign              Installs  Spend  CPI   LTV   ROAS║
║  ─────────────────────────────────────────────────────────────────║
║  Google Ads   GG006_PBC_ROW_ROAS      8,234  $1.2K  $0.15 $0.21 1.44x║
║  Google Ads   GG005_PBC_T1_adROAs     2,852  $889   $0.31 $0.18 0.58x║
║  Unity Ads    UN003_PBC_iOS_US        3,156  $4.2K  $1.33 $0.42 0.32x║
║  Facebook     FB004_TH_AND_IN_MAI    12,456  $2.8K  $0.22 $0.01 0.04x║
║  Facebook     FB003_HS_AND_T2         8,234  $1.9K  $0.23 $0.00 0.00x║
║  Unity Ads    UN002_TH_AND_IN_AEO     1,234  $567   $0.46 $0.05 0.11x║
║  ...                                                               ║
║                                                                    ║
║  🟢 Green = Profitable (ROAS > 1.0)                               ║
║  🔴 Red = Losing Money (ROAS < 0.7)                               ║
║                                                                    ║
╠════════════════════════════════════════════════════════════════════╣
║  [Filters]  Channel: [All ▼]  Campaign: [Search...]  Sort: [ROAS ▼]║
╚════════════════════════════════════════════════════════════════════╝

Navigation: [ << Executive Summary ]  [ Data Quality >> ]
```

---

# DASHBOARD 3: DATA QUALITY REPORT

```
╔════════════════════════════════════════════════════════════════════╗
║  DATA QUALITY ISSUES & OUTLIERS                                    ║
╠════════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  ⚠️  CRITICAL ISSUES IDENTIFIED                                    ║
║                                                                    ║
║  1. ATTRIBUTION ERRORS:                                            ║
║     • 3 Google Ads campaigns with ROAS > 10x (likely bugs)        ║
║     • $1,879 revenue (35% of Google total) needs review           ║
║                                                                    ║
║  2. FACEBOOK ZERO-REVENUE PROBLEM:                                 ║
║     • 120 campaigns (54%) generated $0 revenue                     ║
║     • $8,873 wasted on non-performing campaigns                    ║
║     • High-volume campaigns are the worst performers               ║
║                                                                    ║
║  3. DATA MATURITY:                                                 ║
║     • 608 rows (23%) have <30 days of data                        ║
║     • These are excluded from D30 ROAS calculations                ║
║                                                                    ║
╠════════════════════════════════════════════════════════════════════╣
║  OUTLIER CAMPAIGNS FLAGGED FOR REVIEW                              ║
║                                                                    ║
║  Channel     Campaign           Date      Installs  Spend   ROAS  ║
║  ─────────────────────────────────────────────────────────────────║
║  Google Ads  GG006_PBC_ROW     12/05/25      49    $7.91  223.3x 🔴║
║  Google Ads  GG005_PBC_T1      09/25/25      10    $6.39   17.5x 🔴║
║  Google Ads  GG006_PBC_ROW     10/02/25       6    $0.05   14.4x 🔴║
║                                                                    ║
║  ⚠️ These campaigns need attribution investigation                 ║
║                                                                    ║
╠════════════════════════════════════════════════════════════════════╣
║  FACEBOOK ZERO-REVENUE PATTERN                                     ║
║                                                                    ║
║  Campaign                    Installs    Spend    Revenue          ║
║  ──────────────────────────────────────────────────────────────   ║
║  FB013_PBC_AND_T1_adROAs       2,456    $1,234   $0.00  🔴        ║
║  FB004_TH_AND_IN_MAI           1,892     $876    $0.00  🔴        ║
║  FB002_HS_AND_US_adROAs        1,567     $723    $0.00  🔴        ║
║  FB012_PBC_iOS_MAI             1,234     $567    $0.00  🔴        ║
║  ...                                                               ║
║                                                                    ║
║  Pattern: High-volume campaigns = Zero revenue                     ║
║  Possible causes: Bot traffic, fraud, or severe targeting issues   ║
║                                                                    ║
╠════════════════════════════════════════════════════════════════════╣
║  📋 ACTION ITEMS                                                   ║
║                                                                    ║
║  ☐ Review Google outlier campaigns with data team                 ║
║  ☐ Investigate Facebook for click fraud / bot traffic             ║
║  ☐ Pause top 20 zero-revenue Facebook campaigns immediately        ║
║  ☐ Implement fraud detection on high-volume campaigns              ║
║                                                                    ║
╚════════════════════════════════════════════════════════════════════╝

Navigation: [ << Channel Performance ]  [ << Executive Summary ]
```

---

# DASHBOARD 4: INTERACTIVE EXPLORER (Optional)

```
╔════════════════════════════════════════════════════════════════════╗
║  CAMPAIGN EXPLORER - FILTER & ANALYZE                              ║
╠════════════════════════════════════════════════════════════════════╣
║                                                                    ║
║  ┌─────────────────────────────────────────────────────────────┐  ║
║  │ FILTERS                                                      │  ║
║  │                                                              │  ║
║  │ Channel: [✓] All                                            │  ║
║  │          [✓] Google Ads                                     │  ║
║  │          [✓] Facebook                                       │  ║
║  │          [✓] Unity Ads                                      │  ║
║  │          [ ] ASO Keywords                                   │  ║
║  │                                                              │  ║
║  │ Date Range: [Sep 2025 ─────●────────── Jan 2026]           │  ║
║  │                                                              │  ║
║  │ Campaign: [🔍 Search campaigns...]                          │  ║
║  │                                                              │  ║
║  │ Performance: [All ▼]                                         │  ║
║  │   • All Campaigns                                           │  ║
║  │   • Profitable Only                                         │  ║
║  │   • Losing Money                                            │  ║
║  │   • Scale Opportunities                                     │  ║
║  │                                                              │  ║
║  │ Min Installs: [50 ─────●────── 1000]                       │  ║
║  │                                                              │  ║
║  │ [Apply Filters]  [Reset All]                                │  ║
║  └─────────────────────────────────────────────────────────────┘  ║
║                                                                    ║
╠════════════════════════════════════════════════════════════════════╣
║  RESULTS (234 campaigns match your filters)                        ║
║                                                                    ║
║  Campaign Details Table (full width)                              ║
║  [Shows same table as Dashboard 2 but fully interactive]          ║
║                                                                    ║
║  Export: [📊 Download to Excel]  [📄 Export to PDF]              ║
║                                                                    ║
╚════════════════════════════════════════════════════════════════════╝
```

---

# VISUAL DESIGN PRINCIPLES USED

## 1. HIERARCHY
- **Largest:** Key metrics (spend, installs, ROAS)
- **Medium:** Charts and visualizations
- **Smallest:** Detailed tables and filters

## 2. COLOR STRATEGY
```
🟢 Green (#2ecc71)  = Profitable / Good
🟡 Yellow (#f39c12) = Warning / Near break-even  
🔴 Red (#e74c3c)    = Loss / Problem
⚪ Gray (#95a5a6)   = Neutral / Organic
🔵 Blue (#3498db)   = Informational
```

## 3. INFORMATION DENSITY
- Top: Quick scan (KPIs)
- Middle: Analysis depth (charts)
- Bottom: Detail work (tables)

## 4. WHITE SPACE
- Breathing room between sections
- Not cramped or overwhelming
- Professional, not cluttered

## 5. ACTIONABILITY
- Clear recommendations in text
- Color coding guides decisions
- Filters allow self-service exploration

---

# PRESENTATION FLOW

## Slide Through Dashboards Like This:

**Dashboard 1 (30 seconds):**
*"Here's the executive view: unprofitable overall, but Google shows promise."*

**Dashboard 2 (1 minute):**
*"Drilling down, we see Google users are 4x more valuable. The scatter plot shows our campaigns clearly divided into winners and losers."*

**Dashboard 3 (30 seconds):**
*"I also flagged data quality issues - these outliers and zero-revenue campaigns need immediate attention."*

**Dashboard 4 (Optional):**
*"And managers can use this interactive view to explore any campaign in detail."*

---

# MOBILE VIEW CONSIDERATIONS

If viewing on tablets/mobile:

```
┌──────────────────┐
│  KPI 1  │  KPI 2 │  ← Stack in pairs
├──────────────────┤
│  KPI 3  │  KPI 4 │
├──────────────────┤
│  Chart (full)    │  ← Full width
├──────────────────┤
│  Table (scroll)  │  ← Horizontal scroll
└──────────────────┘
```

Set dashboard to **Automatic** sizing for responsive design.

---

# TOOLTIP DESIGN

When hovering over any data point, show:

```
╔═════════════════════════════════╗
║  Campaign: GG006_PBC_ROW_ROAS   ║
║  Date: December 5, 2025         ║
║  ─────────────────────────────  ║
║  Installs: 49                   ║
║  Spend: $7.91                   ║
║  Revenue: $1,766.01             ║
║  ─────────────────────────────  ║
║  CPI: $0.16                     ║
║  LTV: $36.04                    ║
║  ROAS: 223.29x ⚠️               ║
║  ─────────────────────────────  ║
║  Status: Outlier - Review       ║
║  Recommendation: Investigate    ║
╚═════════════════════════════════╝
```

---

# WHAT MAKES THIS DASHBOARD GREAT

✅ **Tells a clear story:** Problem → Root cause → Solution
✅ **Multiple levels:** Executive → Detail → Quality control
✅ **Actionable:** Clear recommendations, not just data
✅ **Interactive:** Managers can explore on their own
✅ **Honest:** Doesn't hide data quality issues
✅ **Professional:** Clean, not cluttered
✅ **Fast:** < 3 seconds to load any view

---

# BEFORE YOU PRESENT

Check that your dashboard has:

1. **Clear hierarchy** - Eye goes to most important info first
2. **Consistent colors** - Same meaning throughout
3. **Working filters** - Everything responds correctly
4. **Readable text** - No truncated labels
5. **Logical flow** - Story makes sense
6. **Key insights highlighted** - Not buried
7. **No errors** - No "Null" or "Cannot compute"
8. **Fast performance** - Loads quickly

---

**This is your target. Now go build it! 🎨**
