# Swish Growth Analytics: Retention, Conversion & Experiments (Power BI, BigQuery SQL)

> **Swish's Paid Social ads keep only 24.7% of new Bengaluru customers for a second month, against 61.0% in Hyderabad and 58.0% in Chennai. The city looks average and the channel looks average. Only the two together show the problem.**

A five-page Power BI dashboard built on a synthetic dataset modeled on Swish, the Bengaluru-based 10-minute food delivery company. It answers the question a Growth/Product Analyst at Swish would be asked: is Swish acquiring and retaining customers efficiently as it expands into new cities? The data was generated in Python, cleaned in BigQuery SQL, and modeled and visualized in Power BI. *(All figures are synthetic. Data covers 1 April 2025 to 15 September 2026.)*

## Background & Overview

Swish runs its own kitchens, sourcing and delivery, and is opening new cities. Its Growth/Product Analyst job ad asks for funnels and cohorts, retention and conversion analysis, experiment design, and tracking of orders, frequency and AOV, for the Product, Growth, Marketing and Operations teams. I built the project directly from that ad: every metric on the dashboard traces to a line in it.

The dashboard has five pages, each answering one question:

- **Growth Overview:** are revenue, orders, AOV, retention and active customers moving the right way?
- **Customer Journey:** where do customers drop off between opening the app and placing an order?
- **Retention & Cohorts:** who comes back for a second month, by signup cohort, city and channel?
- **City Expansion:** how is each city growing, Q2 against Q1 2026?
- **Experiments & A/B Tests:** which of five product tests actually moved its metric?

A one-page summary for a hiring manager is in [Swish_ONE_PAGER.md](Swish_ONE_PAGER.md).

## Executive Summary

Between 1 July and 15 September 2026, Swish delivered 16,860 orders and **₹66.3 lakh** of revenue, up **8.8%** on the equal-length period before. AOV slipped **0.9%** to ₹393, so all of the growth came from order volume, not bigger baskets. Orders depend on customers coming back, so retention was the next place to look. Month+1 retention is **56.8%** for the latest full cohort (August 2026), up 3.5% on July. That average hides one weak spot: **Paid Social in Bengaluru retains 24.7% of customers**, less than half the 52.1% overall rate, and on a large base of 1,079 customers. The sections below trace how that was found.

## Insights Deep-Dive

**1. The problem is invisible until city and channel are crossed.**
Bengaluru's overall Month+1 retention is 44.7% and Paid Social's overall is 42.5%. Each looks a little weak, but neither looks alarming. Crossing them shows Bengaluru × Paid Social at 24.7%, while the same channel holds 61.0% in Hyderabad and 58.0% in Chennai. Referral keeps 60.3% of customers in Bengaluru, so the city itself is not the cause. It is not a small-sample effect either: Bengaluru's Paid Social cohort (1,079 customers) is larger than Hyderabad, Chennai and Pune combined (882).

**2. Bengaluru is the oldest market, and the slowest growing.**
Bengaluru is 27.7% of Q2 2026 orders but grew just 13.6% on Q1, against 51.9% in Chennai, Swish's newest city in the data. Growth rate falls in step with city age across all six cities. That fits a saturated market, where the same ads have been shown to the same audience for longest. The data cannot prove that, and the retention trend by cohort (a next move below) would test it.

**3. The biggest funnel leak is between browsing and adding to cart.**
Of 463,308 app opens, 14.7% end in an order. The weakest step is Browse to Add to Cart, where only 52.2% move on. The funnel is 60.1% → 52.2% → 72.1% → 65.0% at each step.

**4. One-tap reorder nearly doubled conversion in test.**
A home-screen reorder button lifted conversion from 9.0% to 16.5% (+83.9%). It is the smallest of the five tests (715 customers), but a two-proportion z-test gives p ≈ 0.003, so it is very unlikely to be chance. Overall conversion has stayed flat at 14.6% to 14.7% since the test ran, so it is unclear whether the feature shipped or held up at scale.

**5. Referral is the most reliable channel, and there is a tested way to grow it.**
Referral has the highest Month+1 retention overall (64.0%) and in five of six cities. Chennai is the exception, where Partnerships and Influencer score higher. A separate test that doubled referral credit lifted Day 30 retention of referred customers from 42.3% to 48.0% (p ≈ 0.01). That test uses a different retention window from the dashboard's Month+1, so the two sets of figures should not be compared directly.

**6. Not every test worked.**
Two tests show no detectable effect: notification timing (+3.0%, p ≈ 0.77) and the new checkout layout (−8.4%, p ≈ 0.47). The delivery fee waiver moved AOV from ₹265 to ₹271 (+2.3%); I did not test that one for significance, because it needs the spread of order values.

## Recommendation

1. **Cap new Paid Social acquisition in Bengaluru and redirect that budget** to Referral, which keeps 60.3% of customers in the same city, and to newer cities where Paid Social still works.
   - *Owner:* Growth / Performance Marketing
   - *Expected impact:* As an illustration, lifting Bengaluru Paid Social retention to Pune's level (44.9%, the next-lowest city) would mean about 217 more of those 1,079 customers returning for a second month
   - *Metric to track:* Month+1 retention for Bengaluru × Paid Social, by monthly cohort

2. **Confirm whether one-tap reorder shipped.** If not, ship it. If so, compare conversion for customers who use it against those who do not.
   - *Owner:* Product
   - *Expected impact:* The test lifted conversion by 7.5 percentage points (9.0% to 16.5%); the effect at full scale is unknown
   - *Metric to track:* Browse to Add to Cart conversion (currently 52.2%)

3. **Pilot the doubled referral credit in Bengaluru first,** with Month+1 retention as the primary metric so the result reads directly on the dashboard.
   - *Owner:* Marketing / Growth
   - *Expected impact:* In the earlier test, doubling credit lifted Day 30 retention by 5.7 percentage points; Bengaluru's effect is untested
   - *Metric to track:* Month+1 retention of Referral customers in Bengaluru (currently 60.3%)

## Caveats & Assumptions

- **All data is synthetic.** It is modeled on Swish's publicly known business. No real Swish figures are used or implied.
- **The funnel table is an approximation.** It was generated per date, city and channel from its own funnel-shape assumptions, not from the customers in the orders table. Its 68,081 "Order Placed" events sit close to, but do not reconcile with, the 74,125 orders (67,584 delivered). It is good for step-to-step conversion, not for order-level reconciliation.
- **No win-back in the data.** Once a synthetic customer stops ordering they never return, so every cohort's retention falls near zero by month seven. A real business would level off.
- **No marketing spend data.** The Paid Social finding shows where customers stay, not what they cost.
- **Partial current quarter.** Order data ends on 15 September 2026, so the city comparison uses Q2 against Q1 2026. The date slicer defaults to 1 July to 15 September 2026 because a full-history selection leaves every comparison blank (no earlier period to compare against).
- **Significance tests are approximate.** They use the dashboard's rounded rates and were run outside Power BI.
- **Unlabelled column in the city × channel view** = customers with no recorded acquisition channel (2,197 orders), kept as blank rather than guessed.
- **Delivery time** (294 impossible values set to blank) is in the data model but not on the dashboard.

## Data Structure

A star schema with seven dimension tables and five fact tables at different grains, one per kind of analysis the job ad asks for.

- **Facts:** `fact_orders` (one row per order), `fact_order_items` (one row per item in an order), `fact_funnel_event` (one row per funnel step, per date, city and channel), `fact_customer_monthly_activity` (one row per customer per month they were active), `fact_experiment_results` (one row per customer per experiment).
- **Dimensions:** `dim_date`, `dim_city`, `dim_channel`, `dim_customer`, `dim_kitchen`, `dim_menu_item`, `dim_experiment`.
- **Key Measures table:** all DAX measures sit in one dedicated, disconnected table.

```
dim_date ─────┐
dim_city ─────┤
dim_channel ──┼──► fact_orders ──► fact_order_items ◄── dim_menu_item
dim_customer ─┘
dim_customer ──► fact_customer_monthly_activity, fact_experiment_results, fact_funnel_event
dim_date / dim_city / dim_channel ──► fact_funnel_event
dim_date / dim_experiment ──► fact_experiment_results
dim_date ──► fact_customer_monthly_activity
dim_city ──► dim_kitchen
```

`dim_customer` reaches `dim_city` and `dim_channel` through two inactive relationships, switched on inside one measure with `USERELATIONSHIP`. Two active paths would be ambiguous, because the order and funnel tables already connect to those dimensions directly.

**Deliberately messy raw data, cleaned in SQL.** Raw tables included duplicate rows, city names in several spellings (Bangalore, BANGALORE, Banglore), three date formats, missing channels, impossible delivery times, and orders whose total did not match their items. The cleaning script deduplicated, mapped city spellings through a lookup table, parsed all three date formats, set impossible values to blank instead of deleting real orders, and flagged mismatched order totals in their own table. The cleaned model holds 74,125 unique orders and 12,000 unique customers.

## Tools & Skills

Python (seeded synthetic data generation) · BigQuery SQL (`CREATE TABLE AS SELECT`, lookup tables, `SAFE.PARSE_DATE`, deduplication, reconciliation flags) · Power BI · DAX (dynamic equal-length prior periods, `USERELATIONSHIP` for role-playing dimensions, variables, dynamic format strings) · star-schema modeling · cohort retention and funnel analysis · A/B test readout and two-proportion z-tests · stakeholder-oriented dashboard design

## Screenshots

**Growth Overview:** revenue, orders, AOV, Month+1 retention and active customers with change vs. the prior period
![Growth Overview](screenshots/overview.png)

**Customer Journey:** conversion by funnel step and by month
![Customer Journey](screenshots/funnel.png)

**Retention & Cohorts:** cohort retention curves and Month+1 retention by city and channel
![Retention & Cohorts](screenshots/retention.png)

**City Expansion:** orders and revenue by city, Q2 vs Q1 2026
![City Expansion](screenshots/city-growth.png)

**Experiments & A/B Tests:** Control vs Treatment on each test's own metric
![Experiments & A/B Tests](screenshots/experiments.png)

---

*Built from Swish's publicly listed Growth/Product Analyst role. All data is synthetic and modeled on Swish's publicly known business (a Bengaluru-based 10-minute food delivery company). No real Swish figures are used or implied, and this project is not affiliated with or endorsed by Swish.*
