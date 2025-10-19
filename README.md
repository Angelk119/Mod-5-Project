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
| **Experience Quality** | Average GB/session | Proxy for connection depth and digital inclusion value. |
| **Engagement Growth** | Δ GB/week or Δ Sessions/week | Tracks uptake and sustained usage across time. |

---

## 5. Feature Engineering

| Feature | Formula | Rationale |
|----------|----------|-----------|
| **Total TB** | `TB Downloaded + TB Uploaded` | Measures full data volume for engagement tracking. |
| **Heavy Usage Week** | `gb_per_session ≥ 75th percentile` | Identifies weeks of above-average engagement. |
| **Activation Wait** | `Activation Date – Installation Date` | Quantifies deployment efficiency by borough. |
| **Summer Months** | Boolean (June–Aug) | Captures seasonal activity peaks. |
| **Low Users / Long Session** | Composite binary feature | Detects kiosks with deep but sparse use (potential underserved areas). |

---

## 6. Funnel Analysis

### Funnel Steps

1. **Step 1:** Base — Unique clients per month  
2. **Step 2:** Sessions by new users  
3. **Step 3:** Total GB transferred  
4. **Step 4:** GB per unique client  

### Key Metrics
- Step-to-step conversion and overall GB per client computed monthly.  
- Largest drop-off: between **sessions → total GB**, indicating limited deep engagement despite session growth.  
- Funnel visualization saved to:  
  `/images/linknyc_funnel_latest_month.png`  

---

## 7. Cohorts & Retention Analysis
- Cohorts built **quarterly**, tracking weeks within each cohort.  
- Later cohorts (post-activation) show higher **average GB/session**, suggesting improving adoption over time.  
- Retention measured via “heavy usage” recurrence at 1-week and 4-week intervals.  
- Cohort heatmaps show gradual improvement in engagement depth rather than pure user count.

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
