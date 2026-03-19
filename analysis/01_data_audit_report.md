# 🚨 MOBILE GAME COMPANY DATA AUDIT REPORT - CRITICAL FINDINGS

**Case Study Question 1: Campaign Performance Assessment**  
**Prepared for Marketing Analyst Interview**

---

## 📊 EXECUTIVE SUMMARY

**Dataset Overview:**
- **2,685 rows** covering September 1, 2025 - January 15, 2026 (136 days)
- **4 main channels**: Paid UA (Facebook, Google Ads, Unity Ads, Applovin), Organic, ASO Keywords, Unknown Devices
- **$52,845 total spend** across paid campaigns
- **72,785 total installs** (paid + organic)

**🔴 KEY FINDING: The game is LOSING MONEY**
- Average paid ROAS: **0.71x** (losing 29% on every dollar spent)
- Only **Google Ads is profitable** (1.71x ROAS)
- Average D30 LTV: **$0.52** vs Average CPI: **$1.35** (61% gap)

---

## ⚠️ DATA QUALITY ISSUES & ANOMALIES

### 1. **CRITICAL: "Google Organic Search" is Mislabeled**

**What it actually is:** App Store Optimization (ASO) keyword tracking

**Evidence:**
- 1,236 rows labeled as "Google Organic Search"
- "Campaign_network" field contains search terms: "bubble shooter pro", "puzzle blocks classic", etc.
- Only 1,600 total installs across 1,236 rows (1.3 installs/day average)
- These are **app store search keywords**, not web search traffic

**Impact on Tableau:** 
- ✅ DO label this as "ASO Keywords" or "App Store Search" in your dashboard
- ✅ DO treat as a separate acquisition channel from true organic
- ❌ DON'T combine with general organic traffic

---

### 2. **144 Rows with Zero Installs (5.4%)**

**Breakdown:**
- 137 rows = "Unknown Devices" channel (legacy ad revenue tracking)
- 7 rows = Failed paid campaigns (spent money, got 0 installs)

**Failed Campaigns:**
| Channel | Campaign | Date | Spend |
|---------|----------|------|-------|
| Google Ads | GG005_PBC_AND_T1_adROAs_D7 | 2025-09-10 to 09-14 | $71.48 |
| Unity Ads | UN002_TH_AND_IN_AEO_D1Retention | 2025-11-22 | $0.03 |
| Unity Ads | UN004_PBC_AND_Tier1_MAI_Automated | 2025-10-09 | $0.75 |

**Action:** These should be **EXCLUDED** from CPI/ROAS calculations or flagged as "tracking errors"

---

### 3. **Unknown Devices Channel (137 rows)**

**What it is:** Ad revenue from existing users without install attribution

**Characteristics:**
- 0 installs, $0 cost, $0 IAP revenue
- Only ad revenue ($10,000+ total)
- Daily entries from 2025-09-01 to 2026-01-15

**Impact:** 
- This is **residual revenue** from the existing user base
- Should be tracked separately in dashboard as "Unattributed Ad Revenue"
- ❌ DO NOT include in campaign ROAS calculations

---

### 4. **90% of Rows Have $0 Revenue**

**Why this is NORMAL:**
- 2,430 rows with installs but $0 revenue (90.5%)
- Mobile games typically have 1-5% payer conversion rates
- Most users are ad-monetized only

**Reality Check:**
- Only ~10% of daily cohorts generate ANY IAP revenue
- Ad revenue is present for most cohorts (more consistent)
- This is typical F2P mobile game behavior

---

### 5. **Incomplete Data for Recent Cohorts**

**Finding:** 608 rows (22.6%) have < 30 days of maturity

**Impact:**
- Revenue appears to "drop to zero" after certain days
- 41 rows show revenue decreases (actually incomplete data)
- Average age of "dropped" cohorts: 14.2 days

**Action for Tableau:**
- ✅ Add filter: "Days Since Install >= 30" for accurate D30 metrics
- ✅ Add calculated field: "Data Maturity" to flag incomplete cohorts
- ✅ Use color coding: Green = mature (30+ days), Yellow = partial data

**Calculated Field:**
```
Data Maturity = TODAY() - [Day]
Is Mature = IF [Data Maturity] >= 30 THEN "Mature" ELSE "Incomplete" END
```

---

## 📈 CAMPAIGN PERFORMANCE INSIGHTS

### **Paid Channel Performance (Mature Cohorts Only)**

| Channel | Installs | Spend | Revenue D30 | Avg ROAS | Status |
|---------|----------|-------|-------------|----------|--------|
| **Google Ads** | 11,086 | $3,103 | $5,309 | **1.71x** | ✅ **PROFITABLE** |
| Unity Ads | 25,099 | $19,806 | $6,606 | 0.33x | 🔴 **LOSING 67%** |
| Facebook | 36,000 | $14,952 | $1,627 | 0.11x | 🔴 **LOSING 89%** |
| Applovin | 1,300 | $205 | $50 | 0.24x | 🔴 **LOSING 76%** |

### **Key Metrics**
- **Average CPI:** $1.35
- **Average D30 LTV:** $0.52
- **Average ROAS:** 0.71x
- **Median ROAS:** 0.20x (most campaigns performing even worse)

---

## 🎯 RECOMMENDATIONS FOR TABLEAU DASHBOARD

### **1. EXCLUDE or SEGMENT These Rows:**

✅ **Create filters to handle:**
```
Exclude: [Installs] = 0 (144 rows)
Exclude: [Channel] = "Unknown Devices" for campaign analysis
Segment: [Channel] = "Google Organic Search" → Rename to "ASO Keywords"
Filter: [Data Maturity] >= 30 days for D30 metrics
```

### **2. CREATE These Calculated Fields:**

```tableau
// Clean Total Revenue
[Total Revenue D30] = [revenue_total_cal_d30] + [ad_revenue_total_d30]

// CPI with null handling
[CPI] = IF [Installs] > 0 THEN [network_cost] / [Installs] ELSE NULL END

// ROAS with null handling  
[ROAS D30] = IF [network_cost] > 0 THEN [Total Revenue D30] / [network_cost] ELSE NULL END

// LTV per Install
[LTV D30] = IF [Installs] > 0 THEN [Total Revenue D30] / [Installs] ELSE NULL END

// Profitability Flag
[Is Profitable] = IF [ROAS D30] >= 1 THEN "Profitable" 
                  ELSEIF [ROAS D30] >= 0.7 THEN "Break-even Range"
                  ELSE "Losing Money" END

// Data Maturity
[Days Since Install] = TODAY() - [Day]
[Is Mature Data] = IF [Days Since Install] >= 30 THEN "Mature (30+ days)" ELSE "Incomplete" END

// Channel Cleanup
[Channel Clean] = IF [Channel] = "Google Organic Search" THEN "ASO Keywords"
                  ELSEIF [Channel] = "Unknown Devices" THEN "Unattributed"
                  ELSE [Channel] END
```

### **3. DASHBOARD STRUCTURE:**

**Page 1: Executive Overview**
- Big numbers with color coding (Red < 0.7, Yellow 0.7-1.2, Green > 1.2):
  - Total Spend, Total Installs, Avg ROAS, Total Revenue
- Performance by Channel (Bar chart showing only profitable channels highlighted)
- Profitability Alert: "Only Google Ads is profitable - Consider pausing other channels"

**Page 2: Channel Deep Dive**
- Scatter plot: CPI vs LTV (with break-even diagonal line)
- Daily trend with mature data only
- Network-level performance table (sorted by ROAS)

**Page 3: Cohort Analysis**
- Revenue curves by channel (mature cohorts)
- Time to profitability analysis
- Payback period distribution

**Page 4: ASO Performance**
- Keyword performance table
- ASO vs Paid UA comparison
- Organic install trends

---

## 🎤 TALKING POINTS FOR INTERVIEW

### **When Presenting Your Dashboard:**

1. **Start with the bad news:**
   - "The data shows the game is currently unprofitable with 0.71x ROAS"
   - "This is a soft launch, so this is actually valuable data for decision-making"

2. **Highlight the actionable insight:**
   - "Google Ads is the only profitable channel at 1.71x ROAS"
   - "I'd recommend pausing or drastically reducing Facebook and Unity spend"

3. **Show data sophistication:**
   - "I noticed 'Google Organic Search' is actually ASO keyword data, so I separated it"
   - "I filtered out incomplete cohorts to ensure accurate D30 LTV calculations"

4. **Demonstrate business acumen:**
   - "The D30 LTV of $0.52 vs CPI of $1.35 suggests we need to either:"
     - "Improve monetization (increase IAP or ad placements)"
     - "Reduce CPI through better targeting"
     - "Extend analysis to D60/D90 to capture longer-tail revenue"

5. **Propose next steps:**
   - "I'd want to analyze which campaign creatives/audiences drive Google Ads' profitability"
   - "We should explore why Facebook has such poor performance - creative fatigue?"
   - "Consider A/B testing different monetization strategies"

---

## 📊 METRIC DEFINITIONS (For Reference)

| Metric | Formula | Good Benchmark |
|--------|---------|----------------|
| CPI | Spend / Installs | < $2.00 for casual games |
| D30 LTV | (IAP Rev + Ad Rev) / Installs | > CPI for profitability |
| ROAS | Total Revenue / Spend | > 1.0 (break-even) |
| ROI % | (Revenue - Spend) / Spend × 100 | > 20% for scaling |
| Payback Period | Days until Rev >= Spend | < 30 days ideal |

---

## ✅ FINAL CHECKLIST FOR TABLEAU

- [ ] Rename "Google Organic Search" → "ASO Keywords"
- [ ] Exclude Unknown Devices from campaign ROAS
- [ ] Filter out rows with 0 installs (except for separate analysis)
- [ ] Add "Data Maturity" indicator to all time-based metrics
- [ ] Highlight Google Ads as the winning channel
- [ ] Show Facebook/Unity as loss-making channels
- [ ] Include date range slider (default to mature cohorts)
- [ ] Add calculated ROAS with color coding
- [ ] Create profitability tiers (Profitable / Near Break-even / Losing)
- [ ] Add tooltips explaining each metric
- [ ] Include benchmark reference lines (ROAS = 1.0, CPI = $1.50)

---

## 🎯 EXPECTED INTERVIEW QUESTIONS & ANSWERS

**Q: "What's the biggest issue you see with this campaign?"**  
A: "The overall ROAS of 0.71x means we're losing money. Only Google Ads is profitable. I'd recommend immediately pausing or restructuring Facebook and Unity campaigns which are losing 89% and 67% respectively."

**Q: "How would you improve performance?"**  
A: "Three approaches: 1) Scale Google Ads since it's working, 2) Analyze what makes Google Ads successful (creative, targeting, placement) and apply learnings to other channels, 3) Consider improving in-game monetization since $0.52 D30 LTV is low for the genre."

**Q: "Are there any data quality issues?"**  
A: "Yes, several: 1) 'Google Organic Search' is mislabeled ASO data, 2) 144 rows with zero installs should be excluded, 3) 22% of data is immature (< 30 days) which affects D30 calculations, 4) 'Unknown Devices' represents unattributed ad revenue and should be tracked separately."

**Q: "What would you want to investigate next?"**  
A: "1) Campaign-level performance within Google Ads to identify top performers, 2) Creative analysis to see which ad formats work, 3) Geographic performance - maybe certain markets are more profitable, 4) D60/D90 LTV to see if longer retention improves economics."

---

**Good luck with your interview! This level of analysis shows you're thinking like a senior analyst, not just creating pretty charts.** 🚀
