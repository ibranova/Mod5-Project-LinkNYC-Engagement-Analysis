# Digital Access in NYC: The LinkNYC 5G Analysis
*Analyzing LinkNYC 5G Engagement: How the 5G Rollout Influenced Public Wi-Fi Usage Across New York City*
## Project Owner: [Ayema Qureshi](https://www.linkedin.com/in/ayema-qureshi-901287187/) & [Ibrahima Diallo](https://www.linkedin.com/in/ibranova/)
### Project Management Board: [Trello](https://mod5-prj.atlassian.net/jira/core/projects/MOD5/board?filter=&groupBy=status)
## Overview
- This project analyzes LinkNYC’s weekly Wi-Fi kiosk usage data to uncover trends in engagement, activation, and retention following the introduction of 5G kiosks in New York City. The goal is to determine whether overall usage and engagement quality have improved since the 5G rollout and to identify when audiences are most active.
- Our stakeholder is the City Advertising and Partnerships Team, which collaborates with NYC’s Office of Technology and Innovation (OTI) and CityBridge to manage ad placements and sponsorships on LinkNYC kiosks. They care about understanding how and when New Yorkers engage with the kiosks to optimize ad exposure, sponsorship timing, and return on investment.
- How has LinkNYC engagement evolved since the 5G rollout, and when do usage patterns create the strongest opportunities for advertising and sponsorship value?
  - A successful outcome reveals whether 5G improved engagement consistency and depth, identifies seasonal or behavioral usage patterns, and provides actionable insights that help stakeholders plan targeted ad campaigns aligned with audience activity.
## Data Sources
1. LinkNYC_Weekly_Usage__Updated (2020–Present): This dataset tracks week-by-week Wi-Fi usage metrics, including the number of unique users, total sessions, and total data (GB) transferred. It serves as the core dataset for this analysis and is used to identify engagement patterns, seasonal fluctuations, and retention trends since the 5G rollout. Our analysis focuses primarily on data from July 2022 – Present, when LinkNYC began expanding 5G kiosks across the city.
2. LinkNYC_Kiosk_Locations (2015–Present): This dataset provides contextual information about kiosk placement across New York City, including borough, neighborhood, latitude, and longitude. While not merged directly with the usage data (per project guidelines), it supports spatial interpretation, helping frame engagement insights in relation to kiosk density, neighborhood coverage, and borough-level equity considerations.
## Exploratory Data Analysis (EDA)
### Location Dataset – EDA Summary
The LinkNYC Kiosk Locations dataset provides details about every kiosk installed across New York City, including its installation date, borough, status, zoning type, and whether it’s part of the newer Link5G expansion.

**Objective:** To understand the geographic distribution, installation patterns, and contextual factors behind LinkNYC’s physical presence and assess how equitably kiosks are distributed across boroughs and zoning types. These insights support the City Advertising & Partnerships Team, who rely on kiosk density and placement data to optimize ad visibility and community reach.
**Findings:**

- The dataset contains ~4,000 kiosks, with boroughs listed as: Manhattan, Bronx, Brooklyn, Queens, and Staten Island.
- A few fields had missing values (IXN_Corner, PPT_ID, Legacy_ID), but these mostly identify internal records rather than physical location data, so they were left as-is.
- No major duplicates by kiosk ID were found after removing placeholder or test entries.

  **Missingness Check:**
  - About 8–10% missing values across secondary columns like PPT_ID and IXN_Corner.
  - These are internal identifiers, not critical for spatial or analytical insights.
      - Decision: Retained them rather than dropping rows, since removal wouldn’t impact borough-level trends.
  - *Rule of thumb:* missing data < 5–10% in non-essential fields → keep it!
 
  ### Geographic Distribution
Analyzed the count of kiosks by borough to understand spatial equity.
**Findings:** 
- Manhattan dominates, unsurprisingly, due to higher foot traffic and ad density.
- Brooklyn and Queens follow closely — showing recent expansion under Link5G.
- Bronx shows gradual growth, aligning with city efforts to improve outer-borough connectivity.
- Staten Island has the fewest kiosks (less foot traffic, fewer ad opportunities).
<img width="684" height="454" alt="Screenshot 2025-10-15 at 10 26 16 PM" src="https://github.com/user-attachments/assets/69e6f475-3553-415e-ad90-cbbaf52932bd" />

### Zoning & Land Use Insights

Used the Zoning column to interpret urban context:
- C = Commercial zones:  found near business corridors and ad-heavy streets.
- M = Manufacturing zones: fewer kiosks (industrial areas have low foot traffic).
- R = Residential zones: growing presence, aligning with digital inclusion goals.
- P = Parks/Public spaces: rare, but present near landmarks.

Interpretation: Most kiosks cluster in commercial and mixed-use corridors, ideal for ad visibility. Residential and public zone coverage is improving under Link5G, signaling a stronger equity focus

**Installation Trends Over Time**
Plotted installation counts by year to identify rollout patterns.
<img width="569" height="477" alt="Screenshot 2025-10-15 at 10 30 20 PM" src="https://github.com/user-attachments/assets/c4d2b583-7315-46cc-84b9-cba929e6a142" />

Findings:
- Major rollout between 2016–2018 (initial LinkNYC launch).
- Noticeable dip around 2020 (COVID impact).
- Rebound starting 2022, coinciding with Link5G expansion.
  - The outlier year (like 1971?!) was identified — likely a data entry error or default placeholder. Let’s be real — 5G in 1972? No LinkNYC kiosk existed back when disco was popping.
 
**Link5G vs Original Kiosks**
Flagged kiosks that were newly installed or replaced during the **Link5G expansion (2022–Present)**. Mapped both categories to show replacement trends:
- **Original LinkNYC kiosks (2016–2021)** → concentrated in Manhattan.
- **Link5G kiosks (2022–Present)** → focus shifted toward Bronx, Queens, and Brooklyn.
![UniqueClientPerWeek](/images/kiosk_distribution_map.png)

📍This shift reflects a policy pivot toward equity: bringing better coverage and ad reach to underserved areas.

**Geographic Density (Mapping)**
<img width="953" height="383" alt="Screenshot 2025-10-15 at 10 37 40 PM" src="https://github.com/user-attachments/assets/849fd9ef-e4f1-4ff8-bb6a-61d72ef15589" />

**Findings:**
- The brightest clusters appear in Manhattan, confirming it hosts the majority of kiosks. This aligns with its commercial density, tourism, and high advertising visibility.
- Outer boroughs (Bronx, Brooklyn, Queens) show more scattered coverage, indicating uneven access to LinkNYC services.
- Staten Island has very few kiosks, reflecting lower infrastructure investment in lower-density regions.

**Why it matters:** This heatmap provides a clear visual baseline for equity analysis — it helps identify which areas are oversaturated (potential ad opportunities) versus underserved (potential expansion zones).

### Weekly usage Dataset – EDA Summary
- The dataset contains **weekly system-level summaries** of LinkNYC kiosk activity, including sessions, unique clients, average session length, and total data transferred (TB downloaded/uploaded).
- After cleaning and parsing the Report Ending (weekly starting on Sundays) column, a consistent weekly time series was built for trend and seasonality analysis.
- **Usage patterns show steady weekly engagement**, with spikes during certain weeks — suggesting external factors (e.g., tourism or weather) affect kiosk demand.
- **Unique clients** (weekly active users) and sessions rise and fall together, confirming that total engagement is driven mostly by user reach rather than session frequency alone.

![UniqueClientPerWeek](/images/UniqueClientPerWeek.png)

- **Data transfer (TB downloaded/uploaded)** trends closely follow session counts, indicating sessions remain consistent in average intensity across time.
![UniqueClientPerWeek](/images/AVG_dataUsage_per_Week.png)

- **Outlier weeks** (detected via IQR) correspond to potential event-driven surges, worth tagging for future KPI or campaign analysis.
- No strong anomalies in session length were found, though some small fluctuations suggest seasonal behavior.

### Key Takeaways

- Engagement follows **seasonal and event-driven patterns**, implying that user activity is influenced by city dynamics(weather, events).
- **Unique Clients (WAU)** is the best measure of weekly reach.
- **Sessions per User** and **GB per Session** provide insight into engagement intensity.
- **Rolling averages and event flags** will help smooth noise and identify meaningful usage shifts.
- This dataset forms the foundation for building KPIs like **Weekly Active Users, Engagement Frequency,** and **Network Data Volume per User**.

## KPIs

To measure how effectively LinkNYC kiosks drive engagement, reach, and modernization, we defined four core KPIs aligned with the project goals and stakeholder priorities. Each KPI connects directly to the **City Advertising & Partnerships team’s** need to understand both **audience visibility** and **interaction depth**, i.e., when and where kiosks deliver the greatest advertising and digital access value.

**Activation: “When does engagement begin?”**

**Definition**: A week is considered activated when both the number of unique clients and the number of sessions are above the 75th percentile of weekly activity.
- `unique_clients ≥ 323,242`
- `number_of_sessions ≥ 5,632,313`

**What it measures**: High-visibility weeks where many people are connecting to the network — representing broad audience reach and ad impression potential.
**Why it matters**: The City’s advertising partners value **volume and visibility**. A large number of active users and sessions translates to more ad exposure opportunities and signals strong kiosk utilization across boroughs.

**Conversion: “How many interactions become meaningful?”**

**Definition**: Measures the share of sessions that qualify as heavy-usage sessions (data-heavy interactions). A session-week is marked as heavy usage if:
- `GB_per_session ≥ 0.035 GB` (75th percentile of weekly GB/session)
**Formula**: conversion_rate = heavy_usage_sessions / total_sessions
**What it measures**: The depth of engagement-how many sessions are rich, content-heavy interactions rather than quick connections.
**Why it matters**: Heavy-usage sessions indicate users are spending more time on the kiosk, resulting in longer ad visibility and higher-quality interactions. For advertisers, this helps distinguish between **reach (activation)** and **retention of attention (conversion)**.

**Retention: “Do users come back?”**

**Definition**: Tracks the consistency of weekly activity,  how many “activated” weeks remain active over time.
A week is considered retained if kiosk activity remains strong across multiple weeks (e.g., at least 2 active weeks within any 4-week period).

**What it measures**: The persistence of user engagement over time — how consistently kiosks maintain high activity.

**Why it matters**: Retention reflects **sustained audience interest and reliability of engagement**, showing whether kiosks continue to attract users after the initial connection surge. It also aligns with the City’s goal of **sustained digital inclusion**, not just one-time use.

**Experience / Quality Metric: “How rich is the engagement?”**

**Definition**: Measures the intensity and quality of interactions through
- `GB per session` → Average data used per session (depth of use)
- `Sessions per user` → Frequency of repeat connections (loyalty/intensity)

**What it measures**: The “depth” of engagement — whether sessions are meaningful and repeated, reflecting consistent user value.

**Why it matters**: High-quality experiences mean users find value in the service, leading to stronger trust, repeat usage, and more meaningful exposure for advertisers. For LinkNYC, this bridges technical success (fast 5G connectivity) with human outcomes (continued engagement).

## Feature Engineering
To move beyond raw counts of users and sessions, we engineered a set of analytical features that capture the **quality, timing**, and **consistency** of LinkNYC engagement. Each was designed to uncover why usage rises or falls and when advertisers should act to maximize visibility.

**Sessions per User**
This feature measures how frequently individuals reconnect within a week (`number_of_sessions / number_of_unique_clients`). It helps distinguish whether engagement is broad (many users connecting once) or deep (fewer users connecting repeatedly). Tracking this over time reveals shifts in user behavior — for instance, whether outreach campaigns or seasonal factors lead to more repeat engagement.

**GB per Session**
We created this to capture the depth of engagement, calculated as average gigabytes transferred per session (`(tb_downloaded + tb_uploaded) * 1000 / number_of_sessions)`. Higher values indicate richer activity, such as streaming or downloading, while lower values represent quick browsing or basic use. Monitoring GB per session alongside total sessions helps identify **when and where engagement is most meaningful**, informing whether ad-heavy or high-data kiosks align with high-traffic periods.

**Heavy Usage Indicator**
To separate meaningful engagement from casual use, we engineered a binary feature `heavy_usage_week`, which flags weeks where users consumed above-average data (`GB per session ≥ 0.035`). This feature reframes engagement from “How many people connected?” to “When were people truly engaged?” — identifying periods when users are actively using LinkNYC long enough to view digital ads or sponsored content.

**Seasonal Interaction Term**
Because stakeholder value depends heavily on timing, we created an interaction term `summer_heavy_interaction`, combining the heavy-usage flag with a summer indicator (June–August). This tested whether engagement intensity increases during warmer months when foot traffic and outdoor activity are higher. The analysis showed a mild but consistent lift in engagement during summer, suggesting that *seasonal ad windows* such as summer and holiday quarters provide the best opportunities for campaign visibility.

Together, these features transformed raw engagement data into actionable signals for both technical analysis and stakeholder decision-making. They reveal not just how many people use LinkNYC, but when engagement peaks, how deeply users interact, and what contextual factors drive those behaviors.

### Funnel Analysis
**Goal**
To measure how weekly LinkNYC activity progresses from **broad audience reach → meaningful engagement → sustained usage**, through a sequential funnel that highlights where engagement drop-offs occur. This structure connects directly to our KPIs:
- **Activation → Conversion → Retention → Quality Experience**

**Stage Definitions and Logic**
| Stage | Name | Metric & Condition | Purpose |
| :---: | :--- | :--- | :--- |
| **2** | **Active Weeks** | `df["number_of_sessions"] > 0` | Confirms **continuous activity**—the network is never idle post-5G rollout. |
| **3** | **High-Reach Weeks** | `df["number_of_unique_clients"] >= users_50` | Captures periods of **strong reach** using the 50th percentile as threshold—includes typical-to-high weeks, not just spikes. |
| **4** | **Engaged Weeks** | `df["GB_per_session"] >= 0.035` | Defines **meaningful engagement**; 0.035 GB/session $\approx$ 75th percentile, representing sessions with richer ad exposure. |
| **5** | **Retained Weeks** | `(df["stage4_engaged"].astype(int).rolling(window=2,min_periods=2).sum().shift(-1) >= 2)` | Proxy for **short-term retention**—engagement that persists week-to-week (requires two consecutive engaged weeks). |

**Threshold Adjustment Rationale**
While my initial threshold for engagement quality was based on the **75th percentile of GB per session (~0.035 GB)**, I adjusted it for this funnel analysis to create a **more interpretable, true funnel shape**.

In early iterations, using the original threshold produced almost no variation between stages — the counts stayed flat or even increased, making it difficult to visualize meaningful drop-offs. This wasn’t a coding issue but a data distribution problem: LinkNYC’s weekly metrics are aggregated, not user-level, so engagement thresholds apply unevenly across time.

To preserve the logic of a funnel (each stage representing a stricter subset of the previous one), we **lowered the GB/session threshold** for the engaged stage. This adjustment allowed the visualization to behave as expected — progressively narrowing — while still maintaining analytical integrity.

**Results**
| Stage | Count | Step → Step Conversion | Overall Conversion (from Stage 1) | Interpretation |
| :---: | :---: | :---: | :---: | :--- |
| **Stage 1: All Weeks** | 165 | 1.000 | 1.000 | **Baseline:** total 165 weeks of 5G data. |
| **Stage 2: Active Weeks** | 165 | 1.000 | 1.000 | Every week shows user activity—**continuous engagement**. |
| **Stage 3: High-Reach Weeks** | 83 | 0.503 | 0.503 | About **half of all weeks** exceed median traffic—consistent audience exposure. |
| **Stage 4: Engaged Weeks** | 65 | 0.783 | 0.394 | Roughly **78% of high-traffic weeks** deliver strong session quality. |
| **Stage 5: Retained Weeks** | 52 | 0.800 | 0.315 | $\approx$ **80% of engaged weeks** repeat engagement the following week—strong short-term retention. |

### Funnel Insights — Understanding LinkNYC’s Engagement Journey
The funnel analysis revealed how LinkNYC usage transitions from broad audience reach to deep, recurring engagement. By mapping weekly data through the stages of Activation, Reach, Engagement, and Retention, we gained a clearer view of how consistent and meaningful user interactions are across the city since the rollout of Link5G.

The first takeaway is the network’s remarkable consistency — there was no drop-off between “All Weeks” and “Active Weeks.” Every single week in the dataset recorded network activity was recorded, demonstrating that LinkNYC operates as an **always-on digital infrastructure**. This reliability is a major strength for the City’s Advertising and Partnerships team, as it guarantees constant visibility for any campaigns running through the kiosks.

Moving into the next stage, about half of all weeks qualified as “high-reach” weeks, where unique user counts exceeded the citywide median. This pattern reflects **seasonal and event-driven engagement**, likely corresponding with periods of increased tourism, warmer weather, or large-scale city events. These peaks highlight strategic opportunities for advertisers to time their campaigns during quarters when foot traffic and audience visibility are at their highest.

Among those high-reach weeks, roughly 78% also showed strong engagement depth, defined by high data consumption per session (≥ 0.035 GB). This suggests that when user activity increases, it’s not limited to quick Wi-Fi checks — people are using LinkNYC for richer digital experiences. These interactions represent the **highest-value ad moments**, when users are stationary, attentive, and exposed to kiosk content for longer durations.

Retention proved equally strong. Approximately 80% of engaged weeks maintained similar levels of engagement the following week, indicating consistent user behavior over time. Even without individual user IDs, this week-over-week pattern points to a **loyal and repeat audience base**. Once users engage, they tend to return, which demonstrates that LinkNYC’s 5G upgrade not only improved speed and accessibility but **also stabilized user retention** across boroughs.

Overall, the funnel shape tells a powerful story. Instead of a steep drop-off, the narrowing is gradual — reflecting a platform that attracts large audiences and keeps them engaged in a sustained, predictable way. LinkNYC’s user journey is not built on fleeting spikes but on steady, recurring activity. This consistency is key for advertisers and city partners alike: it means predictable exposure, dependable performance, and measurable value across seasons.

In short, the funnel shows that LinkNYC has evolved into more than just a network of Wi-Fi kiosks. It represents a **stable engagement ecosystem** that provides both continuous reach and quality interaction. For stakeholders, this translates to a platform where campaigns can reach thousands of New Yorkers each week — not just once, but again and again.

### Cohort and Retention Analysis
In this analysis, each cohort represents a starting week of high engagement, defined as weeks where the average data usage per session exceeded **0.035 GB**. From that point, we tracked how engagement evolved in the following weeks, similar to tracking how groups of users who “activate” together continue to return over time.

Retention was measured by observing whether these “heavy-usage” weeks were followed by additional high-engagement weeks within the next one to four weeks.
- **W1 retention** captured whether engagement persisted in the immediate next week.
- **W4 retention** measured whether another heavy-usage week occurred within the next month.

The results were consistent:
- **W1 Retention**: ~78% of heavy weeks were followed by another high-engagement week.
- **W4 Retention**: ~82% of heavy weeks had another high-engagement week within the next month.

These findings suggest **habit-forming and resilient user behavior**. Once the city experiences a strong week of kiosk activity, it tends to sustain high usage for several weeks afterward. The few dips that do occur are likely tied to external factors such as weather, events, or temporary kiosk outages — all actionable opportunities for targeted reinforcement during slower periods.

From a business perspective, this pattern shows that once engagement peaks, it remains **sticky** — users continue returning, providing dependable visibility for advertisers.

#### Recommendations for Stakeholders:
- City advertisers can leverage these patterns by launching and maintaining campaigns during high-engagement cohorts, ideally sustaining them for **2–4 weeks** to maximize exposure. Historical cohort data can also help forecast future “heavy engagement windows” during major events or seasons. Finally, when engagement declines abruptly, local investigations (e.g., network quality or outreach) can identify and address potential causes.

## RFM Segmentation and ROI Analysis
We applied an **RFM framework** — Recency, Frequency, and Monetary value — to describe the overall health of kiosk engagement over time.
- **Recency (R)** measured how recently the city experienced strong engagement (a heavy-usage week).
- **Frequency (F)** tracked how often those high-engagement weeks occurred.
- **Monetary (M)** represented total data consumed, used as a proxy for attention depth.

Together, these metrics allowed us to identify **which time periods or kiosk clusters deliver the most valuable, sustained engagement**.

- High R, F, and M scores (our “Champions”) reflected periods of persistent, high-value interaction — where users consistently connected and consumed rich content.
- Low recency but high frequency or monetary scores (“Dormant”) indicated areas that were previously strong but may now need reactivation
- Low frequency and monetary scores (“At-Risk”) pointed to underutilized zones — not necessarily due to disinterest, but possibly limited awareness or infrastructure issues.
<img width="702" height="199" alt="Screenshot 2025-10-15 at 9 32 37 PM" src="https://github.com/user-attachments/assets/85c08c1b-6c35-47ff-9811-ac846dc9bb7e" />

This segmentation reframes raw usage into a practical ROI map for city advertisers — highlighting where sponsorships yield the greatest sustained attention and where reinvestment could increase digital inclusion.

#### Recommendations for Stakeholders:

![ScarterPlot](/images/RFM_scatter_plot.png)

- Prioritize `Champion` kiosks or periods for premium ad inventory since they deliver the most consistent audience visibility. Re-engage “Dormant” segments with localized campaigns or technical checks. Use “At-Risk” zones for experimentation and community engagement — pilot equity initiatives, multilingual outreach, or local content to build trust and awareness.

For budget allocation, we recommend combining RFM with a prioritization model like **RICE (Reach, Impact, Confidence, Effort)** to balance ROI-driven investments with equitable distribution

## Equity and Access Considerations
- **Concentration risk**: Focusing only on high-usage kiosks could channel ad revenue and upgrades toward business or tourist areas, widening access gaps in lower-income neighborhoods.
- **Measurement bias**: Low engagement numbers don’t always mean low interest — they can reflect kiosk outages, poor signage, or limited awareness.
- **Language & accessibility**: Non-English-speaking or senior communities may engage less unless kiosks include localized instructions and targeted outreach.
- **Aggregation limits**: City-level metrics reveal trends but can hide kiosk-level issues like outages or uneven performance.

**Summary:**

The combination of cohort, retention, and RFM analyses demonstrates that LinkNYC engagement is both sustained and scalable. Engagement spikes persist for weeks, high-frequency zones deliver reliable exposure, and equity-aware targeting ensures inclusivity alongside profitability. Together, these insights give the City Advertising and Partnerships team data-informed insights for timing, targeting, and optimizing their digital outreach across New York City.

## Ethical and Equity Considerations

This project was designed with an equity lens, since LinkNYC’s original rollout was heavily concentrated in **Manhattan**, while the **5G expansion (from 2022 onward)** aimed to close access gaps across the other boroughs. Our analysis focused on the 5G period because it represents a deliberate effort to create a **more balanced, citywide distribution of connectivity.**

Still, there are important equity nuances to acknowledge. Even as 5G installations expanded coverage, kiosk placement and engagement remain uneven. Manhattan continues to dominate in kiosk density and overall data usage, while outer boroughs like the Bronx and Staten Island are still catching up. This raises questions about whether advertising value and infrastructure investment might still **cluster around high-traffic, high-income, or tourist zones**, potentially sidelining lower-traffic neighborhoods.

From an ethical perspective, it’s important to recognize that **engagement metrics don’t always reflect demand fairly**. Low-usage areas may appear underperforming not because of lack of interest, but due to structural barriers like kiosk outages, visibility, or local awareness. Without multilingual interfaces or tailored outreach, residents in certain communities — particularly non-English-speaking populations — may not engage with kiosks as easily. Although LinkNYC does include accessibility features like hearing loops, braille labels, and TalkBack support, the absence of clear multilingual UI options may still limit inclusivity for some users.

### Limitations and Assumptions
Our analysis faced several technical and methodological limitations:
- **Aggregated Data**: The weekly dataset does not include individual user IDs, preventing user-level tracking. This means we inferred engagement patterns through week-level proxies (e.g., retention based on recurring high-usage weeks) rather than true behavioral cohorts.
- **Unjoinable Datasets**: The usage and location datasets could not be perfectly merged at the kiosk level due to differing ID formats and missing fields. As a result, borough-level trends were analyzed separately from week-level engagement metrics.
- **Assumed Causality**: Because this is observational data, we cannot claim causality — only correlation. For example, higher GB/session values in Manhattan could stem from population density, better coverage, or ad targeting, but we can’t isolate which.
- **Threshold Calibration**: Some thresholds (like the funnel’s GB/session cutoff) were adjusted for interpretability rather than strict statistical significance. This ensured the funnel reflected real behavioral progression rather than data noise.
- **Equity Assumptions**: We assumed that post-2022 installations under the 5G program were more evenly distributed, aligning with city equity goals. While data supports this trend, full validation would require kiosk-level demographic overlays that weren’t available.

Despite these limitations, every decision — from focusing on 5G data to recalibrating funnel thresholds — was made to balance **analytical integrity with fairness and transparency**. Our results aim to tell a realistic story about **how engagement and equity intersect** in public digital infrastructure, not just where the highest numbers appear.

## Key Insights and Recommendations
Across all analyses, LinkNYC engagement during the 5G period (2022–Present) shows a **steady, continuous pattern of use and strong audience retention**. Every week in the dataset recorded active sessions, confirming that the network is consistently in use — there were no inactive weeks or large drop-offs in overall traffic.

The funnel analysis showed a clear, interpretable progression from activation to deep engagement. Roughly half of all weeks reached high traffic levels (“high-reach”), and about 78% of those also achieved strong session quality (“engaged” weeks). Among these engaged periods, around 80% maintained activity the following week — a sign of **stable, recurring user engagement**. These findings highlight that LinkNYC’s audience is not just large but dependable over time.

The retention and cohort analyses confirmed that high-usage periods tend to persist across multiple weeks, suggesting **habitual engagement patterns**. Even without individual user data, week-over-week continuity indicates that once users connect, engagement remains consistent. This stability supports reliable ad exposure windows for the City’s Advertising and Partnerships team.

The RFM segmentation further revealed that engagement is not only active but also **high-value**. Weeks with recent, frequent, and heavy usage formed a “Champion” segment, representing periods of strong attention depth. Other segments — such as “Dormant” or “At-Risk” — showed opportunities for reactivation or outreach. This segmentation reframes raw usage into **actionable insights for sponsorship and outreach planning**.

Taken together, the analyses show that LinkNYC’s post-5G engagement is **continuous, high-quality, and retention-driven**. For stakeholders, this means that ad and sponsorship campaigns can be scheduled confidently around predictable engagement cycles rather than short-lived peaks.

**Recommendations:**
- Maintain long-term or multi-week sponsorships to match LinkNYC’s sustained engagement patterns.
- Monitor both session volume and quality (GB/session) to evaluate ad visibility and attention, not just reach.
- Use RFM segmentation to identify strong-performing periods for ad placement and reactivation strategies for lower-engagement weeks.
- Continue analyzing retention and funnel data to measure the impact of future 5G expansions or kiosk upgrades.

## Next Steps

If additional, more granular data were available — such as kiosk-level or user-level identifiers — future work could include:
- **Cohort analysis by borough or kiosk cluster** to identify geographic variations in engagement or retention.
- **User-level retention metrics** to measure true repeat usage rather than week-level recurrence.
- **Integration with demographic or event data** to explore how local context (weather, tourism, or city events) influences engagement patterns.
- **Equity impact analysis** to assess whether new kiosk installations under Link5G achieve balanced coverage across neighborhoods.

## Repository Structure
```
/linknyc-growth
├─ data/
│  ├─ clean
│  │  ├─ LinkNYC_Weely_Usage_cleaned_2022-current.csv
│  │  ├─ LinkNYC_Weely_Usage_cleaned_20251008-current.csv
│  │  ├─ LinkNYC_location_cleaned_20251008.csv
│  ├─ raw
│  │  ├─ LinkNYC_Weely_Usage_20251008.csv
│  │  ├─ LinkNYC_location_20251008.csv
├─ notebooks/
│  ├─ 01_eda_and_kpis.ipynb
│  ├─ 02_features_and_funnel.ipynb
│  └─ 03_cohorts_rfm_and_roi.ipynb
│  └─ 01_eda_weeklyUsage_and_kpis.ipynb
│  └─ EDA_2022-current.ipynb
├─ images/      
│  └─ funnel_official.png
│  └─ kiosk_installations_per_year.png
│  └─ density_mapbox.png
│  └─ weekly_session_overTime.png
│  └─ average_usage_perSession.png
│  └─ RFM_scatter_plot.png
│  └─ Cohort_heatmap.png
│  └─ AVG_dataUsage_per_week.png
├─ pitch_deck.pdf
└─ README.md
```





























































 








  





