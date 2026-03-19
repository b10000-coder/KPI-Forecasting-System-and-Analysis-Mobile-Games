# 📋 TABLEAU CALCULATED FIELDS - COPY-PASTE SHEET
## All formulas in one place for quick setup

---

## ✅ SETUP CHECKLIST
- [ ] Data imported
- [ ] Date field formatted correctly
- [ ] Data Source Filter: [Installs] > 0
- [ ] All calculated fields created (17 total)
- [ ] Test calculations with sample data

---

## 1️⃣ REVENUE METRICS (3 fields)

### Total Revenue D30
```tableau
[revenue_total_cal_d30] + [ad_revenue_total_d30]
```

### Total Revenue D7
```tableau
[revenue_total_cal_d7] + [ad_revenue_total_d7]
```

### Total Revenue D14
```tableau
[revenue_total_cal_d14] + [ad_revenue_total_d14]
```

---

## 2️⃣ EFFICIENCY METRICS (6 fields)

### CPI
```tableau
IF SUM([installs]) > 0 THEN 
    SUM([network_cost]) / SUM([installs])
ELSE 
    NULL 
END
```

### ROAS D30
```tableau
IF SUM([network_cost]) > 0 THEN 
    SUM([Total Revenue D30]) / SUM([network_cost])
ELSE 
    NULL 
END
```

### ROAS D7
```tableau
IF SUM([network_cost]) > 0 THEN 
    SUM([Total Revenue D7]) / SUM([network_cost])
ELSE 
    NULL 
END
```

### LTV D30
```tableau
IF SUM([installs]) > 0 THEN 
    SUM([Total Revenue D30]) / SUM([installs])
ELSE 
    NULL 
END
```

### ROI %
```tableau
IF SUM([network_cost]) > 0 THEN
    ((SUM([Total Revenue D30]) - SUM([network_cost])) / SUM([network_cost])) * 100
ELSE
    NULL
END
```

### Profit Loss D30
```tableau
SUM([Total Revenue D30]) - SUM([network_cost])
```

---

## 3️⃣ DATA QUALITY FIELDS (5 fields)

### Days Since Install
```tableau
DATEDIFF('day', [day], TODAY())
```

### Is Mature (30+ days)
```tableau
[Days Since Install] >= 30
```
**Type:** Boolean

### Data Maturity
```tableau
IF [Days Since Install] >= 30 THEN 
    "Mature (30+ days)"
ELSEIF [Days Since Install] >= 14 THEN
    "Partial (14-29 days)"
ELSE 
    "Incomplete (<14 days)"
END
```

### Channel Clean
```tableau
IF [channel] = "Google Organic Search" THEN 
    "ASO Keywords"
ELSEIF [channel] = "Unknown Devices" THEN 
    "Unattributed"
ELSE 
    [channel]
END
```

### Data Quality Flag
```tableau
IF [ROAS D30] > 10 THEN 
    "Outlier (ROAS > 10x)"
ELSEIF [Installs] < 10 THEN 
    "Low Volume (<10 installs)"
ELSE 
    "Valid"
END
```

---

## 4️⃣ FILTERS (1 field)

### Is Paid Campaign
```tableau
[network_cost] > 0
```
**Type:** Boolean

---

## 5️⃣ PERFORMANCE CLASSIFICATION (2 fields)

### Performance Status
```tableau
IF SUM([network_cost]) = 0 THEN 
    "Organic"
ELSEIF [ROAS D30] >= 1.5 THEN 
    "Highly Profitable"
ELSEIF [ROAS D30] >= 1.0 THEN 
    "Profitable"
ELSEIF [ROAS D30] >= 0.7 THEN 
    "Near Break-even"
ELSE 
    "Losing Money"
END
```

### Recommendation
```tableau
IF SUM([network_cost]) = 0 THEN 
    "N/A - Organic"
ELSEIF [ROAS D30] >= 1.2 AND SUM([installs]) >= 500 THEN 
    "Scale Up"
ELSEIF [ROAS D30] >= 1.2 AND SUM([installs]) < 500 THEN 
    "Test & Scale"
ELSEIF [ROAS D30] >= 0.8 THEN 
    "Optimize"
ELSE 
    "Pause/Reduce"
END
```

---

## 🎨 COLOR PALETTE (Copy hex codes)

### Channels:
- Google Ads: `#3498db` (Blue)
- Facebook: `#34495e` (Dark Blue)
- Unity Ads: `#9b59b6` (Purple)
- ASO Keywords: `#27ae60` (Green)
- Organic: `#95a5a6` (Light Gray)

### Performance:
- Profitable: `#2ecc71` (Green)
- Near Break-even: `#f39c12` (Orange)
- Losing: `#e74c3c` (Red)

---

## 🔢 NUMBER FORMATTING

| Field | Format | Example |
|-------|--------|---------|
| network_cost | Currency, $#,##0 | $1,234 |
| installs | Number, #,##0 | 1,234 |
| CPI | Currency, $0.00 | $1.23 |
| LTV D30 | Currency, $0.00 | $0.45 |
| ROAS D30 | Custom, 0.00x | 1.23x |
| ROI % | Percentage, 0% | -25% |
| Profit Loss | Currency, $#,##0 | -$567 |

---

## 📊 MUST-HAVE FILTERS

Apply to all worksheets:
1. `Is Paid Campaign = True` (for cost-related analysis)
2. `Is Mature (30+ days) = True` (for D30 metrics)
3. `Data Quality Flag = "Valid"` (excludes outliers)

Apply to dashboards as user controls:
1. `Channel Clean` (multi-select dropdown)
2. `day` (date range slider)
3. `Campaign Network` (search box)

---

## 🎯 REFERENCE LINES TO ADD

### For ROAS charts:
- **Value:** 1.0
- **Label:** "Break-even"
- **Style:** Dashed, Gray

### For CPI charts:
- **Value:** 1.50
- **Label:** "Target CPI"
- **Style:** Dashed, Blue

### For LTV charts:
- **Value:** 2.00
- **Label:** "Healthy LTV"
- **Style:** Dashed, Green

---

## 🚨 COMMON ERRORS & FIXES

### "Cannot mix aggregate and non-aggregate arguments"
**Fix:** Add SUM() around measures:
```tableau
❌ [Total Revenue D30] / [network_cost]
✅ SUM([Total Revenue D30]) / SUM([network_cost])
```

### "The field is a duplicate"
**Fix:** You created two fields with the same name. Delete one.

### "Null values everywhere"
**Fix:** 
1. Check if filters are too restrictive
2. Verify calculations have ELSE NULL or ELSE 0
3. Check data source for missing values

### "ROAS showing as 0.00x instead of NULL"
**Fix:** Change format to allow special values:
- Format → Pane → Special Values → Show at default position

---

## ⚡ QUICK BUILD ORDER (60 minutes)

**If short on time, build in this order:**

1. **Calculated Fields (20 min):**
   - Total Revenue D30
   - CPI
   - ROAS D30
   - LTV D30
   - Channel Clean
   - Is Mature (30+ days)

2. **KPI Scorecards (10 min):**
   - Spend, Installs, Revenue, ROAS
   - Format as big numbers with color

3. **Channel Bar Chart (10 min):**
   - Rows: Channel Clean
   - Columns: ROAS D30
   - Color by value
   - Add break-even reference line

4. **Campaign Table (10 min):**
   - Rows: Channel, Campaign Network
   - Columns: All key metrics
   - Conditional formatting on ROAS

5. **Dashboard Assembly (10 min):**
   - KPIs at top
   - Bar chart in middle
   - Table at bottom
   - Add filters

---

## 💡 PRO TIPS

1. **Save often!** Tableau crashes happen.

2. **Test calculations:**
   - Create a simple table view
   - Drag field to Text
   - Check if numbers make sense
   - Spot-check against Excel

3. **Use aliases:**
   - Right-click field → Aliases
   - Rename "Facebook" → "Facebook Ads"
   - Makes dashboard more professional

4. **Duplicate worksheets:**
   - Right-click sheet → Duplicate
   - Faster than rebuilding from scratch

5. **Hide unused fields:**
   - Right-click field → Hide
   - Keeps data pane clean

6. **Group campaigns:**
   - Select values in viz
   - Right-click → Group
   - Create campaign types (e.g., "iOS", "Android")

---

## 🎤 PRESENTATION SCRIPT (Copy This)

**Opening:**
*"I've built a dashboard that shows campaign performance across four key areas: overall profitability, channel comparison, campaign-level detail, and data quality issues."*

**Key Finding 1:**
*"The game is currently unprofitable at 0.71x ROAS. However, Google Ads is performing at 1.1x after removing outliers, showing the game has potential with the right targeting."*

**Key Finding 2:**
*"Facebook is the main problem - 54% of campaigns have zero revenue despite high volume, suggesting either fraud or severe targeting issues. I've flagged these for investigation."*

**Key Finding 3:**
*"The root cause is user quality, not cost. Google users monetize at $0.18 per install versus Facebook's $0.05 - a 4x difference."*

**Recommendation:**
*"Immediate actions: Scale Google Ads, pause underperforming Facebook campaigns, and investigate the zero-revenue pattern for potential fraud."*

---

## 📱 DASHBOARD NAVIGATION

Create buttons using text objects:

```
[ Executive Summary ] → [ Channel Deep Dive ] → [ Data Quality ]
```

Use dashboard actions:
- Action Type: Go to URL
- URL: `<Dashboard Name>`

---

## ✅ FINAL QUALITY CHECK

Before presenting:
- [ ] All calculations return expected values
- [ ] No "Null" showing (unless appropriate)
- [ ] Colors are consistent across all views
- [ ] Titles are clear and action-oriented
- [ ] Tooltips provide additional context
- [ ] Filters work and are intuitive
- [ ] Dashboard loads quickly (< 3 seconds)
- [ ] Key insights jump out visually
- [ ] Data quality issues are flagged
- [ ] Recommendations are clear

---

**You're ready! Go build your dashboard! 🚀**
