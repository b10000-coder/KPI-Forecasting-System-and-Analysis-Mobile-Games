# 📊 TABLEAU DASHBOARD BUILD GUIDE
## Mobile Game Company Marketing Analyst Case Study - Step by Step

**Goal:** Create a dashboard that helps marketing managers assess campaign performance and make data-driven decisions.

**Time Estimate:** 2-3 hours for full build

---

# 🎯 DASHBOARD STRATEGY

## The Story We're Telling:
1. **Overall Health:** Game is unprofitable but shows potential
2. **Channel Performance:** Google Ads works, Facebook doesn't
3. **Data Quality:** Some issues need attention
4. **User Quality:** Major difference between channels
5. **Actionable Insights:** Clear next steps

## Dashboard Structure (5 tabs):
1. **Executive Summary** - Key metrics at a glance
2. **Channel Performance** - Deep dive by channel
3. **Campaign Analysis** - Granular campaign-level data
4. **Data Quality** - Outliers and issues flagged
5. **Cohort Analysis** - Revenue curves over time

---

# PART 1: DATA PREPARATION (15 minutes)

## Step 1.1: Import Data into Tableau

1. Open Tableau Desktop
2. Click **"Connect to Data"** → **"Text file"**
3. Navigate to `Mobile Game Company_sample_data.csv`
4. Click **"Open"**

### What You'll See:
- Data Source tab opens
- Preview of your data (2,685 rows, 65 columns)

## Step 1.2: Initial Data Check

**In the Data Source view:**

1. Check that **"day"** column is recognized as **Date**
   - If not: Click the icon above "day" → Change to **Date**

2. Check numeric columns:
   - `network_cost` → Number (decimal)
   - `installs` → Number (whole)
   - All revenue columns → Number (decimal)

3. **Rename the data source:**
   - Right-click "Mobile Game Company_sample_data" → Rename to **"Campaign Data"**

## Step 1.3: Create Data Source Filter (IMPORTANT!)

This prevents bad data from polluting your analysis:

1. Click **"Add"** next to "Filters" (top right)
2. Add filter: `[Installs] > 0`
   - This excludes 144 rows with zero installs
3. Click **"OK"**

**Why:** Zero-install rows break CPI/LTV calculations and aren't real campaigns.

---

# PART 2: CREATE CALCULATED FIELDS (45 minutes)

Click **"Sheet 1"** to enter the worksheet view. We'll create all calculated fields before building visualizations.

## Step 2.1: Core Revenue Metrics

### 1. Total Revenue D30
```tableau
[revenue_total_cal_d30] + [ad_revenue_total_d30]
```
**Where:** Right-click in Data pane → **Create Calculated Field**
**Name:** `Total Revenue D30`

### 2. Total Revenue D7
```tableau
[revenue_total_cal_d7] + [ad_revenue_total_d7]
```
**Name:** `Total Revenue D7`

### 3. Total Revenue D14
```tableau
[revenue_total_cal_d14] + [ad_revenue_total_d14]
```
**Name:** `Total Revenue D14`

## Step 2.2: Efficiency Metrics

### 4. CPI (Cost Per Install)
```tableau
IF SUM([installs]) > 0 THEN 
    SUM([network_cost]) / SUM([installs])
ELSE 
    NULL 
END
```
**Name:** `CPI`
**Note:** This is an aggregate calculation (uses SUM)

### 5. ROAS D30
```tableau
IF SUM([network_cost]) > 0 THEN 
    SUM([Total Revenue D30]) / SUM([network_cost])
ELSE 
    NULL 
END
```
**Name:** `ROAS D30`

### 6. ROAS D7
```tableau
IF SUM([network_cost]) > 0 THEN 
    SUM([Total Revenue D7]) / SUM([network_cost])
ELSE 
    NULL 
END
```
**Name:** `ROAS D7`

### 7. LTV D30 (per user)
```tableau
IF SUM([installs]) > 0 THEN 
    SUM([Total Revenue D30]) / SUM([installs])
ELSE 
    NULL 
END
```
**Name:** `LTV D30`

### 8. ROI Percentage
```tableau
IF SUM([network_cost]) > 0 THEN
    ((SUM([Total Revenue D30]) - SUM([network_cost])) / SUM([network_cost])) * 100
ELSE
    NULL
END
```
**Name:** `ROI %`

### 9. Profit/Loss
```tableau
SUM([Total Revenue D30]) - SUM([network_cost])
```
**Name:** `Profit Loss D30`

## Step 2.3: Data Quality Fields

### 10. Days Since Install
```tableau
DATEDIFF('day', [day], TODAY())
```
**Name:** `Days Since Install`

### 11. Is Mature Data
```tableau
[Days Since Install] >= 30
```
**Name:** `Is Mature (30+ days)`
**Data Type:** Boolean

### 12. Data Maturity Label
```tableau
IF [Days Since Install] >= 30 THEN 
    "Mature (30+ days)"
ELSEIF [Days Since Install] >= 14 THEN
    "Partial (14-29 days)"
ELSE 
    "Incomplete (<14 days)"
END
```
**Name:** `Data Maturity`

### 13. Channel Clean (rename ASO)
```tableau
IF [channel] = "Google Organic Search" THEN 
    "ASO Keywords"
ELSEIF [channel] = "Unknown Devices" THEN 
    "Unattributed"
ELSE 
    [channel]
END
```
**Name:** `Channel Clean`

### 14. Is Outlier (flag suspicious data)
```tableau
IF [ROAS D30] > 10 THEN 
    "Outlier (ROAS > 10x)"
ELSEIF [Installs] < 10 THEN 
    "Low Volume (<10 installs)"
ELSE 
    "Valid"
END
```
**Name:** `Data Quality Flag`

### 15. Paid Campaign Filter
```tableau
[network_cost] > 0
```
**Name:** `Is Paid Campaign`
**Data Type:** Boolean

## Step 2.4: Performance Classification

### 16. Profitability Status
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
**Name:** `Performance Status`

### 17. Scale Recommendation
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
**Name:** `Recommendation`

---

# PART 3: BUILD WORKSHEETS (90 minutes)

Now we build individual visualizations that we'll combine into dashboards.

## WORKSHEET 1: Executive KPI Cards

**Purpose:** Show key metrics at a glance

### Create 6 separate worksheets (one per metric):

#### KPI 1: Total Spend
1. Create new worksheet: **"KPI - Spend"**
2. Drag `network_cost` to **Text** on Marks card
3. Change aggregation to **SUM**
4. Click **Text** → Format → **Number** → **Currency (Custom)** → `$0.0K`
5. Add filter: `Is Paid Campaign = True`
6. Add filter: `Is Mature (30+ days) = True`
7. Format:
   - Font size: **36**
   - Alignment: Center
   - Add label below: "Total Spend"

#### KPI 2: Total Installs
1. Create new worksheet: **"KPI - Installs"**
2. Drag `installs` to **Text**
3. SUM aggregation
4. Format: Number (Custom) → `0.0K`
5. Same filters as above
6. Font size: **36**

#### KPI 3: Total Revenue
1. Create new worksheet: **"KPI - Revenue"**
2. Drag `Total Revenue D30` to **Text**
3. SUM aggregation
4. Format: Currency → `$0.0K`
5. Same filters
6. Font size: **36**

#### KPI 4: Average CPI
1. Create new worksheet: **"KPI - CPI"**
2. Drag `CPI` to **Text**
3. Format: Currency → `$0.00`
4. Same filters
5. Font size: **36**

#### KPI 5: Average ROAS
1. Create new worksheet: **"KPI - ROAS"**
2. Drag `ROAS D30` to **Text**
3. Format: Number (Custom) → `0.00x` (add "x" in suffix)
4. **Color coding:**
   - Drag `ROAS D30` to **Color**
   - Edit colors → **Stepped Color** → 3 steps
   - Red: 0.0 - 0.7
   - Orange: 0.7 - 1.0
   - Green: 1.0+
5. Same filters
6. Font size: **36**

#### KPI 6: ROI %
1. Create new worksheet: **"KPI - ROI"**
2. Drag `ROI %` to **Text**
3. Format: Number → `0%`
4. Color code (Red: <-30%, Orange: -30 to 0, Green: >0)
5. Same filters
6. Font size: **36**

---

## WORKSHEET 2: Channel Performance Bar Chart

**Purpose:** Compare ROAS across channels

1. Create new worksheet: **"Channel ROAS Comparison"**

### Build the Chart:
1. **Rows:** `Channel Clean` (dimension)
2. **Columns:** `ROAS D30` (measure)
3. **Mark Type:** Bar
4. **Color:** 
   - Drag `ROAS D30` to Color
   - Edit colors: Red-Yellow-Green diverging
   - Center: 1.0 (break-even)
5. **Label:** 
   - Drag `ROAS D30` to Label
   - Format: `0.00x`
6. **Sort:** By `ROAS D30` descending

### Add Filters:
- `Is Paid Campaign = True`
- `Is Mature (30+ days) = True`
- `Data Quality Flag = "Valid"` (excludes outliers)

### Add Reference Line:
1. Right-click Y-axis → **Add Reference Line**
2. Value: **1.0**
3. Label: "Break-even"
4. Line: Dashed, Gray

### Format:
- Title: "ROAS by Channel (Mature Campaigns, D30)"
- Hide field labels
- Show value labels on bars

---

## WORKSHEET 3: Channel Performance Matrix (Scatter)

**Purpose:** Visualize efficiency (CPI vs LTV)

1. Create new worksheet: **"Channel Efficiency Matrix"**

### Build the Chart:
1. **Columns:** `CPI` (measure)
2. **Rows:** `LTV D30` (measure)
3. **Mark Type:** Circle
4. **Detail:** `Campaign Network` (so each campaign is a dot)
5. **Color:** `Channel Clean`
6. **Size:** `installs` (SUM)
7. **Label:** `Campaign Network` (show for large circles only)

### Add Filters:
- `Is Paid Campaign = True`
- `Is Mature (30+ days) = True`
- `Data Quality Flag = "Valid"`

### Add Reference Lines:
1. **Diagonal break-even line:**
   - Right-click chart → **Add Reference Line** (both axes)
   - Type: Distribution
   - This is tricky - instead use Trend Line:
   - Analysis menu → **Trend Lines** → **Show Trend Lines**
   - Edit trend line → Linear, force through origin

2. **Average lines:**
   - Add vertical reference line at average CPI
   - Add horizontal reference line at average LTV

### Quadrant Labels (add annotations):
- Top Left: "⭐ Best Performers" (high LTV, low CPI)
- Top Right: "Premium Quality" (high LTV, high CPI)
- Bottom Left: "Test Zone" (low LTV, low CPI)
- Bottom Right: "⚠️ Stop These" (low LTV, high CPI)

---

## WORKSHEET 4: Daily Performance Trend

**Purpose:** Track installs and spend over time

1. Create new worksheet: **"Daily Trend"**

### Build the Chart:
1. **Columns:** `day` (continuous date)
2. **Rows (create 2 measures):**
   - `installs` (SUM)
   - `network_cost` (SUM)
3. **Mark Type:** Line
4. **Color:** `Channel Clean`

### Make Dual Axis:
1. Right-click second measure → **Dual Axis**
2. Right-click right Y-axis → **Synchronize Axis**

### Add Filters:
- `Is Paid Campaign = True`
- `Channel Clean` (show as multi-select filter on dashboard)

### Format:
- Title: "Daily Installs & Spend by Channel"
- Legend: Bottom
- Date format: MM/DD

---

## WORKSHEET 5: Campaign Deep Dive Table

**Purpose:** Detailed campaign-level metrics

1. Create new worksheet: **"Campaign Table"**

### Build the Table:
1. **Rows (in this order):**
   - `Channel Clean`
   - `Campaign Network`
2. **Columns (measures, in this order):**
   - `installs` (SUM)
   - `network_cost` (SUM)
   - `CPI`
   - `Total Revenue D30` (SUM)
   - `LTV D30`
   - `ROAS D30`
   - `ROI %`
   - `Profit Loss D30`
   - `Recommendation`

### Format as Table:
1. **Mark Type:** Text
2. Drag each measure to **Text** on Marks card
3. Format numbers:
   - Installs: `#,##0`
   - Cost/Revenue: `$#,##0`
   - CPI/LTV: `$0.00`
   - ROAS: `0.00x`
   - ROI: `0%`

### Add Conditional Formatting:
1. For `ROAS D30`:
   - Right-click → **Format**
   - Pane tab → **Shading**
   - Based on value: Red (<0.7), Yellow (0.7-1.0), Green (>1.0)

2. For `Profit Loss D30`:
   - Red for negative, Green for positive

### Add Filters:
- `Is Paid Campaign = True`
- `Is Mature (30+ days) = True`
- Minimum installs: Create parameter → `>= 50`

### Sort:
- Default sort by `ROAS D30` descending

---

## WORKSHEET 6: Cohort Revenue Curves

**Purpose:** Show how revenue grows over time

**Note:** This requires pivoting the data (advanced - optional for interview)

If you want to include this:

1. You'll need to use **Tableau Prep** or Excel to pivot revenue columns first
2. Alternative: Show D7, D14, D30 as separate bars instead

### Simpler Version (3 bars per channel):
1. Create new worksheet: **"Revenue Progression"**
2. **Rows:** `Channel Clean`
3. **Columns:** 
   - `LTV D7` (calculated as Total Revenue D7 / installs)
   - `LTV D14`
   - `LTV D30`
4. **Mark Type:** Bar (side by side)
5. **Color:** Different for each time period
6. **Label:** Show values

---

## WORKSHEET 7: Data Quality Dashboard

**Purpose:** Flag issues for the team

1. Create new worksheet: **"Outliers"**

### Build the Table:
1. **Rows:**
   - `Channel Clean`
   - `Campaign Network`
   - `day`
2. **Columns:**
   - `installs` (SUM)
   - `network_cost` (SUM)
   - `ROAS D30`
   - `Data Quality Flag`

### Add Filter:
- `Data Quality Flag != "Valid"` (show ONLY outliers)

### Format:
- Highlight `ROAS D30 > 10` in red
- Add annotation: "These campaigns need review"

---

## WORKSHEET 8: Facebook Zero-Revenue Analysis

**Purpose:** Show the Facebook problem specifically

1. Create new worksheet: **"FB Quality Issue"**

### Build the Chart:
1. **Rows:** `Campaign Network` (filtered to Facebook only)
2. **Columns:** `installs` (SUM)
3. **Color:** `Total Revenue D30` (SUM)
   - Use Red-Green scale
   - 0 = Dark Red
   - >0 = Green

### Add Filter:
- `Channel Clean = "Facebook"`
- `Is Mature (30+ days) = True`

### Sort:
- By installs descending (shows high-volume zero-revenue campaigns at top)

---

# PART 4: ASSEMBLE DASHBOARDS (30 minutes)

Now we combine worksheets into 5 dashboard tabs.

## DASHBOARD 1: Executive Summary

**Size:** Desktop (1200 x 800)

### Layout:
```
┌────────────────────────────────────────────────────┐
│  CAMPAIGN PERFORMANCE - EXECUTIVE SUMMARY          │
├──────────┬──────────┬──────────┬──────────────────┤
│  KPI 1   │  KPI 2   │  KPI 3   │  KPI 4-6         │
│  Spend   │ Installs │ Revenue  │  CPI/ROAS/ROI    │
├──────────┴──────────┴──────────┴──────────────────┤
│  Channel ROAS Comparison (Bar Chart)               │
│  (Worksheet 2)                                     │
├───────────────────────────────────────────────────┤
│  KEY INSIGHTS TEXT BOX:                            │
│  - Game is unprofitable at 0.71x ROAS             │
│  - Only Google Ads is profitable (1.3x cleaned)    │
│  - Recommend pausing Facebook campaigns            │
└───────────────────────────────────────────────────┘
```

### Build Steps:
1. Create new **Dashboard** → Name: "1. Executive Summary"
2. Set size: **Desktop (1200 x 800)**
3. Drag KPI worksheets to top row (horizontal container)
4. Drag "Channel ROAS Comparison" below
5. Add **Text** object at bottom with key insights
6. Add **Filter** object: Date range slider

### Add Interactivity:
1. Dashboard menu → **Actions** → **Add Action** → **Filter**
2. Source: "Channel ROAS Comparison"
3. Target: All worksheets
4. Behavior: Click on a bar to filter entire dashboard

---

## DASHBOARD 2: Channel Deep Dive

**Size:** Desktop (1200 x 900)

### Layout:
```
┌────────────────────────────────────────────────────┐
│  Channel Performance Analysis                      │
├──────────────────────┬────────────────────────────┤
│  Channel Efficiency  │  Daily Trend               │
│  Matrix (Scatter)    │  (Line Chart)              │
│                      │                            │
├──────────────────────┴────────────────────────────┤
│  Campaign Deep Dive Table                          │
│  (Sortable, with conditional formatting)           │
└────────────────────────────────────────────────────┘
```

### Build Steps:
1. Create new Dashboard: "2. Channel Performance"
2. Top row: Scatter plot (left) + Line chart (right)
3. Bottom: Campaign table (full width)
4. Add filters:
   - Channel selector (multi-select)
   - Date range
   - Minimum installs slider

---

## DASHBOARD 3: Data Quality Report

**Size:** Desktop (1200 x 700)

### Layout:
```
┌────────────────────────────────────────────────────┐
│  Data Quality Issues & Outliers                    │
├────────────────────────────────────────────────────┤
│  SUMMARY STATS:                                    │
│  - 3 campaigns flagged as outliers (ROAS > 10x)   │
│  - 120 Facebook campaigns with zero revenue        │
│  - $8,873 spent on non-performing FB campaigns     │
├────────────────────────────────────────────────────┤
│  Outliers Table (Worksheet 7)                      │
├────────────────────────────────────────────────────┤
│  Facebook Zero-Revenue Analysis (Worksheet 8)      │
└────────────────────────────────────────────────────┘
```

### Build Steps:
1. Create new Dashboard: "3. Data Quality"
2. Add text box at top with summary stats
3. Add outliers table
4. Add Facebook analysis chart
5. Add annotation explaining these need review

---

## DASHBOARD 4: Quick Filters View

**Purpose:** Interactive exploration for managers

1. Create new Dashboard: "4. Interactive Explorer"
2. Add the campaign table as main element
3. Add prominent filters:
   - Channel (multi-select, show all)
   - Date range (slider)
   - Campaign Network (search box)
   - Performance Status (dropdown)
   - Minimum installs (slider: 0-1000)

---

## DASHBOARD 5: Cohort Analysis

**Optional - if you built the revenue curves**

Show D7/D14/D30 progression by channel.

---

# PART 5: FORMATTING & POLISH (20 minutes)

## Step 5.1: Consistent Color Scheme

Apply consistent colors throughout:

1. **Channels:**
   - Google Ads: Blue (#3498db)
   - Facebook: Dark Blue (#34495e)
   - Unity Ads: Purple (#9b59b6)
   - ASO Keywords: Green (#27ae60)
   - Organic: Light Gray (#95a5a6)

2. **Performance:**
   - Profitable: Green (#2ecc71)
   - Near Break-even: Orange (#f39c12)
   - Losing: Red (#e74c3c)

### Apply:
1. Right-click color legend → **Edit Colors**
2. Assign colors manually
3. Click **Assign Palette**

## Step 5.2: Format All Worksheets

For each worksheet:

1. **Remove gridlines:**
   - Format → Lines → Grid Lines → None

2. **Clean titles:**
   - Double-click title
   - Use clear, action-oriented language
   - Example: "Which Channels are Profitable?" not "ROAS by Channel"

3. **Format axes:**
   - Right-click axis → Format
   - Remove unnecessary decimals
   - Add currency symbols
   - Use "K" notation for thousands

4. **Add tooltips:**
   - Click Tooltip on Marks card
   - Include:
     - Campaign name
     - Key metrics
     - Performance status
     - Recommendation

## Step 5.3: Dashboard Formatting

For each dashboard:

1. **Add title banner:**
   - Text object at top
   - Background color: Dark gray (#34495e)
   - Text color: White
   - Font: Bold, 24pt

2. **Add navigation:**
   - Image objects with buttons (or text objects)
   - Link to other dashboards
   - Dashboard → Actions → Go to URL → Dashboard name

3. **Add filter controls:**
   - Make filters prominent
   - Use dropdown for single select
   - Use multi-select with "All" option
   - Add "Reset" button

4. **Add context:**
   - Text boxes explaining what managers should look for
   - Example: "Green = Profitable, Red = Losing Money"

---

# PART 6: FINAL CHECKS (10 minutes)

## Checklist:

### Data Quality:
- [ ] Zero-install rows excluded (Data Source Filter)
- [ ] Outliers flagged or removed in main views
- [ ] ASO renamed from "Google Organic Search"
- [ ] Mature cohorts filter applied (30+ days)

### Calculations:
- [ ] ROAS calculated correctly (revenue / cost)
- [ ] CPI calculated correctly (cost / installs)
- [ ] All revenue includes both IAP + Ad
- [ ] NULL handling in place for zero denominators

### Visualizations:
- [ ] All charts have clear titles
- [ ] Axes are properly formatted
- [ ] Color coding is consistent
- [ ] Reference lines at break-even points
- [ ] Tooltips are informative

### Dashboards:
- [ ] Filters are easy to use
- [ ] Click actions work (filter on select)
- [ ] Navigation between dashboards works
- [ ] Key insights are visible
- [ ] No data "No data" messages (means filters too restrictive)

### Story:
- [ ] Clear progression: Summary → Detail → Quality
- [ ] Main insight is obvious (Google profitable, FB not)
- [ ] Data issues are acknowledged
- [ ] Recommendations are clear

---

# PART 7: INTERVIEW PRESENTATION TIPS

## How to Present Your Dashboard:

### Opening (30 seconds):
*"I built a 4-part dashboard to help assess campaign performance. Let me walk you through the key findings."*

### Dashboard 1 - Executive Summary (1 minute):
*"Starting with the high-level view: We've spent $53K on 72K installs, generating $13.5K in revenue. The overall ROAS is 0.71x, meaning we're losing 29 cents per dollar spent. However, there's a clear winner..."*

[Click on Google Ads bar]

*"Google Ads is profitable at 1.1x ROAS after cleaning outliers, while Facebook is at 0.08x."*

### Dashboard 2 - Channel Performance (1 minute):
*"This scatter plot shows why: Google users monetize at $0.18 per install versus Facebook's $0.05. It's not a cost issue - Facebook's CPI is actually similar. It's a user quality issue."*

[Point to the campaign table]

*"The table lets managers sort by any metric and see which specific campaigns to scale or pause."*

### Dashboard 3 - Data Quality (30 seconds):
*"I also found data quality issues: 3 Google campaigns with suspicious ROAS that need attribution review, and 120 Facebook campaigns with zero revenue despite high volume, suggesting bot traffic or fraud."*

### Closing (30 seconds):
*"My recommendation: Scale Google Ads immediately, investigate Facebook for fraud, and fix those attribution errors. The game has potential - we just need to focus on what's working."*

## Questions They Might Ask:

**Q: "How did you decide what to include?"**
A: "I focused on what marketing managers need to make decisions: profitability by channel, campaign-level detail for optimization, and data quality flags so they know what to trust."

**Q: "Why did you separate the data quality dashboard?"**
A: "Data issues can distort decision-making. I wanted a clean view for strategy while also being transparent about what needs fixing."

**Q: "How would you improve this?"**
A: "With more time, I'd add cohort-based revenue curves, predictive LTV modeling, and creative performance analysis. I'd also automate alerts for campaigns below target ROAS."

---

# BONUS: ADVANCED FEATURES (If You Have Time)

## 1. Parameters for Dynamic Analysis

Create parameter: "Select Time Window"
- Values: D7, D14, D30
- Use in calculated field to switch between timeframes

## 2. LOD Expressions

```tableau
Channel Average ROAS:
{FIXED [Channel Clean] : AVG([ROAS D30])}

Above/Below Average:
IF [ROAS D30] > {FIXED [Channel Clean] : AVG([ROAS D30])} 
THEN "Above Avg" 
ELSE "Below Avg" 
END
```

## 3. Custom SQL for Advanced Users

If you're comfortable with SQL:
1. Data Source → Custom SQL
2. Write query to pre-aggregate data
3. Faster performance for large datasets

## 4. Tooltips with Viz-in-Tooltip

Create small spark line charts that appear on hover.

---

# TROUBLESHOOTING

## Common Issues:

### "Cannot mix aggregate and non-aggregate"
- Fix: Add SUM() around measures in calculated field
- Or: Change to row-level calculation

### "Null values appearing"
- Check if filters are too restrictive
- Verify data isn't actually missing
- Add IFNULL() to calculated fields

### "Chart doesn't update when I change filter"
- Check: Dashboard Actions are set up
- Verify: Worksheets use correct data source
- Reset: Clear all filters and rebuild

### "Colors look wrong"
- Fix: Right-click legend → Edit Colors
- Reverse: Check if scale is backwards
- Assign: Manually assign colors to values

### "Text doesn't fit in dashboard"
- Adjust: Font size down
- Resize: Container size
- Wrap: Enable text wrapping

---

# FINAL DELIVERABLE

Your finished Tableau workbook should contain:

📁 **Mobile Game Company_Marketing_Analysis.twbx**

**Data Sources:** 
- Campaign Data (Mobile Game Company_sample_data.csv)

**Calculated Fields:** 17 fields created

**Worksheets:** 8+ individual visualizations

**Dashboards:** 
1. Executive Summary
2. Channel Performance  
3. Data Quality Report
4. Interactive Explorer

**Story Points (optional):**
- Create a Story that walks through your analysis

---

# TIME ALLOCATION GUIDE

If you have **2 hours:**
- Part 1: Data Prep (15 min)
- Part 2: Calculated Fields (30 min)
- Part 3: Build 4 key worksheets (45 min)
- Part 4: 2 dashboards (20 min)
- Part 5: Polish (10 min)

If you have **3 hours:**
- All of above
- Plus: Advanced features
- Plus: Data quality dashboard
- Plus: Beautiful formatting

If you have **1 hour:**
- Focus on: Executive Summary dashboard only
- Must-haves: KPIs + Channel bar chart + Campaign table
- Skip: Scatter plot, trends, data quality tab

---

**Good luck! You've got this! 🚀**

Remember: The goal isn't perfection - it's showing you can:
1. ✅ Understand business questions
2. ✅ Clean and validate data
3. ✅ Create clear visualizations
4. ✅ Derive actionable insights
5. ✅ Communicate findings

Your depth of data analysis already sets you apart. Now just bring it to life visually!
