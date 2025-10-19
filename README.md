# 🗽 LinkNYC Kiosk Equity & Usage Analysis

## 1. Overview & Business Question
**Business Problem:**  
LinkNYC’s kiosks are concentrated in high-traffic, higher-income areas, leaving low-income neighborhoods underserved. This uneven distribution widens New York City’s digital divide and limits equitable access to free Wi-Fi.  

**Business Question:**  
How can LinkNYC use data-driven insights to expand kiosks in underserved areas, maximize user engagement, and strengthen public digital equity?

---

## 2. Data Sources
- **Dataset 1 — Weekly Usage (NYC Open Data):**  
  [LinkNYC Weekly Usage](https://data.cityofnewyork.us/Social-Services/LinkNYC-Weekly-Usage/ffxx-dfvk)  
  → Metrics on sessions, data transfer, and unique clients by week.  

- **Dataset 2 — Kiosk Locations (NYC Open Data):**  
  [LinkNYC Kiosk Locations](https://data.cityofnewyork.us/City-Government/LinkNYC-Kiosk-Locations/bt4t-8t7c)  
  → Metadata on kiosk installation, activation, type, and borough-level deployment.  

---

## 3. Exploratory Data Analysis (EDA) Summary

### Weekly Usage EDA
- Converted date fields to `datetime`.  
- Normalized numeric fields (TB → GB; sessions per user).  
- Checked for missing weeks, duplicates, and extreme outliers in TB transferred.  
- Identified strong **seasonality** (summer peaks in GB/session).  
- Activation defined via 75th percentile of GB/session.

### Kiosk Location EDA
- Grouped by **borough** and **installation status**.  
- Manhattan holds ~60% of active kiosks; Bronx and Staten Island under 10%.  
- Calculated **average activation wait** (install → live): 61–113 days.  
- Created zoning feature (Residential, Commercial, Manufacturing).  

---

## 4. Key Performance Indicators (KPIs)

| KPI | Definition | Justification |
|------|-------------|---------------|
| **Activation Rate (Quality)** | % of weeks with GB/session ≥ 75th percentile (Z) | Captures meaningful digital engagement, not just volume. |
| **Retention Rate (Weekly)** | % of users returning within 4 weeks | Measures loyalty and long-term impact of access. |

---

## 5. Feature Engineering

### Essential Features

| Feature | Formula | Rationale |
|--------|---------|-----------|
| **Total TB** | `TB Downloaded + TB Uploaded` | Captures the total volume of data usage per week to measure engagement. |
| **Heavy Usage Week** | `True if session GB ≥ activation threshold` | Identifies weeks with unusually high engagement levels. |
| **Low Users / Long Session** | `True if Low Users AND Long Session` | Flags kiosks with few users but long sessions—potential indicators of underserved areas. |
| **Activation Wait** | `Activation Date - Installation Date` | Measures time lag in kiosk deployment, useful for borough-level analysis. |

### Assisting Features

| Feature | Formula | Rationale |
|--------|---------|-----------|
| **TB to GB** | `1 TB = 1024 GB` | Standardizes data units for consistency across features. |
| **Long Session** | `True if avg. session length ≥ 25 minutes` | Identifies kiosks with unusually long user engagement. |
| **Low Users** | `True if weekly users ≤ 250,000` | Highlights kiosks with relatively low foot traffic. |
| **Zoning** | `3-letter zoning code (e.g. "R2A / C1-2")` | Describes the zoning classification where each kiosk is installed. |
| **Summer Months** | `True if report week in June, July, or August` | Captures potential seasonal usage spikes. |


---

## 6. Funnel Analysis

### Funnel Steps

1. **Step 1:** Base amount of users for the month = 1
2. **Step 2:** Increase in sessions from new users  
3. **Step 3:** Increase in data transferred (GB) for sessions for that month  
4. **Step 4:** increase in data transfer(GB) by number of unique clients  

### Key Metrics
- Step-to-step conversion and overall GB per client computed monthly.  
- Largest drop-off: between **sessions → total GB**, indicating limited deep engagement despite session growth.  
- Funnel visualization:
- <img width="265" height="245" alt="Screenshot 2025-10-19 at 1 43 14 PM" src="https://github.com/user-attachments/assets/e8f1d273-d317-4aba-bd7c-bbe49860d00e" /> 

---

## 7. Cohorts & Retention Analysis 
- Cohorts built **quarterly**, tracking weeks within each cohort.  
- Later cohorts (post-activation) show higher **average GB/session**, suggesting improving adoption over time.  
- Retention measured via “heavy usage” recurrence at 1-week and 4-week intervals.  
- Cohort heatmaps show gradual improvement in engagement depth rather than pure user count. 
<img width="630" height="342" alt="Screenshot 2025-10-19 at 1 45 03 PM" src="https://github.com/user-attachments/assets/d2e1da75-f003-4849-a7ab-21db7191e314" />

---

## 8. RFM Segmentation → ROI

### RFM Definitions
- **Recency (R):** Weeks since last heavy-usage week.  
- **Frequency (F):** Count of heavy-usage weeks in 12-week window.  
- **Monetary (M):** Total GB transferred (proxy for data-driven value).  

### Segments
| Segment | Behavior | ROI Implication |
|----------|-----------|----------------|
| **Champions** | Very recent, frequent, high GB | Highest value — maintain performance. |
| **Loyal** | Consistent but moderate recency | Stable ROI — good for ad targeting. |
| **At-Risk** | High past use, declining recency | Re-engagement opportunity. |
| **Dormant** | Low frequency, low GB | Consider kiosk relocation or new awareness push. |

**ROI Estimation:**  
Assuming ~$0.50 advertising value per GB:  
- A 10% uplift in GB/session among “At-Risk” kiosks → ~8–12% incremental ROI.  
- Price/GB proxy (total value ÷ GB) used to assess efficiency improvements post-rollout.

---

## 9. Ethical Considerations

| Area | Risk | Mitigation |
|-------|------|-------------|
| **Equity** | Unequal kiosk placement reinforces digital divide. | Prioritize underserved ZIP codes; use density & income data. |
| **Access** | Public Wi-Fi quality and reach vary by location. | Monitor throughput; deploy upgrades in high-usage zones. |
| **Privacy** | Data aggregation masks individual behavior. | Maintain anonymized metrics and avoid user-level profiling. |
| **Measurement Bias** | Weekly aggregation may smooth out real usage volatility. | Supplement with kiosk-level logs where available. |

---

### 🏁 Summary
Expanding LinkNYC kiosks into underserved communities offers measurable public value:  
- **Higher digital inclusion** through equitable Wi-Fi access.  
- **Improved ROI** via increased GB/session engagement.  
- **Sustained social impact** by aligning urban infrastructure with NYC’s digital equity goals.  
