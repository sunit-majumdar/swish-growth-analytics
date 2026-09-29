# Swish Growth Dashboard: one page for the hiring manager

Built for the Growth/Product Analyst role at Swish. All data is synthetic, generated to match the shape of Swish's business. Every number below is on the dashboard or reproducible from the model behind it.

## What the dashboard shows

**Growth Overview** shows revenue, orders, AOV, Month+1 retention and active customers for any date range, each compared with the equal-length period before it.

**Customer Journey** shows how many customers reach each step from app open to order placed, and how end-to-end conversion has moved month by month.

**Retention & Cohorts** shows how long each signup cohort keeps ordering, and Month+1 retention for every pairing of city and acquisition channel.

**City Expansion** compares Q2 and Q1 2026 orders and revenue for each of Swish's six cities, fastest-growing city first.

**Experiments & A/B Tests** shows Control against Treatment for five tests, each on the metric it was designed to move.

## Three things the data is saying

### 1. Paid Social has stopped working in Bengaluru, and only in Bengaluru.

**What.** Customers acquired through Paid Social in Bengaluru come back for a second month 24.7% of the time. The same channel keeps 61.0% of customers in Hyderabad and 58.0% in Chennai. Every other channel in Bengaluru keeps between 41.5% and 60.3%.

**Why.** From 1 July to 15 September 2026, revenue grew 8.8% while AOV fell 0.9%, so all of the growth came from order count. Orders depend on customers coming back, so retention was the next place to look. By city, Bengaluru is lowest at 44.7%. By channel, Paid Social is lowest at 42.5%. Neither number looks alarming on its own. The problem only shows up when city and channel are crossed. It is not a small-sample effect: Bengaluru's Paid Social cohort is 1,079 customers, more than Paid Social brought in across Hyderabad, Chennai and Pune combined (882).

**So what.** Bengaluru is Swish's oldest and largest market, 27.7% of Q2 orders, and its slowest-growing, with orders up 13.6% against 51.9% in Chennai. The channel bringing in the most new customers there keeps the fewest of them. The likeliest explanation is audience fatigue, since Bengaluru has had the longest exposure to the same ads. The data cannot prove that on its own, and the first next move below tests it.

**Now what.** Growth should cap new Paid Social acquisition in Bengaluru and move that budget to Referral, which keeps 60.3% of customers in the same city, and to the newer cities where Paid Social still performs.

### 2. One-tap reorder nearly doubled conversion in test.

**What.** A one-tap reorder button on the home screen raised conversion from 9.0% to 16.5%, a lift of 83.9% and the largest of the five experiments.

**Why.** The funnel's biggest leak is between browsing and adding to cart, where only 52.2% of customers move on. One-tap reorder lets a returning customer skip browsing entirely. The test was the smallest of the five at 715 customers, but a two-proportion z-test on its rates gives p of about 0.003, so the lift is very unlikely to be chance.

**So what.** For Product, this is the only test in the set with a large and reliable effect. It ran in June and July 2025, yet end-to-end conversion has sat flat at 14.6% to 14.7% since October 2025. Either the feature never shipped, or its effect did not hold at full scale. Both answers change what Product should do next.

**Now what.** Product should confirm whether one-tap reorder shipped. If it did not, ship it. If it did, compare conversion for customers who use it against those who do not.

### 3. Referral is Swish's most reliable channel, and there is a tested way to grow it.

**What.** Referral customers have the highest Month+1 retention of any channel, 64.0% overall, and the highest in five of the six cities, ranging from 60.3% to 67.2%. Chennai is the exception, where Partnerships and Influencer score higher. Separately, a test that doubled referral credit raised retention of referred customers from 42.3% to 48.0%, a 13.5% lift with a z-test p of about 0.01. That test measured Day 30 retention, a different window from the dashboard's Month+1, so the two sets of figures should not be compared directly.

**Why.** Referral holds up in Bengaluru too (60.3%), which rules out the city itself as the cause of the first finding. The weakness belongs to Paid Social in Bengaluru, not to Bengaluru's customers.

**So what.** For Growth and Marketing, Referral is the natural replacement for Bengaluru's Paid Social acquisition, and the incentive test shows the channel responds when credit goes up.

**Now what.** Marketing should run the doubled referral credit in Bengaluru first and track the result on the city by channel retention view.

## Three next moves

1. **Is Bengaluru's Paid Social retention falling cohort after cohort, or has it always been low?** Run Month+1 retention by `cohort_month` for Bengaluru and Paid Social only, from `fact_customer_monthly_activity` joined to `dim_customer` on home city and acquisition channel. A steady decline supports fatigue. A flat low line points to targeting instead, which needs a different fix.
2. **Did one-tap reorder ship, and do the customers who use it convert better?** This needs a feature-usage flag that this dataset does not contain. The first question to the Product team is whether that event is logged.
3. **Does doubled referral credit keep its lift in Bengaluru?** Run the incentive as a Bengaluru-only test, with Month+1 retention as the primary metric so the result reads directly on the dashboard, and compare Treatment against the current 60.3%.

## What generated data cannot tell us

There is no marketing spend or cost data, so acquisition cost and return by channel cannot be calculated. The first finding says where customers stay, not what they cost. Once a synthetic customer stops ordering they never come back, so every cohort's retention falls close to zero by month seven, where a real business would level off. The five experiments ran at different times on different customers, so they are separate readouts, not one combined test. The significance tests were run outside Power BI on the raw experiment counts. The delivery fee waiver's AOV lift (₹265 to ₹271) was tested with a t-test on order values and is real (p below 0.001), though small. Some customers have no recorded acquisition channel and appear as an unlabelled column in the retention view. Order data ends on 15 September 2026, so the current quarter is incomplete and the city comparison uses Q2 against Q1. Delivery time is in the data model but not on the dashboard.

## Synthetic data

Every figure in this dashboard and this note is synthetic, modeled on Swish's publicly known business as a Bengaluru-based 10-minute food delivery company. No real Swish figures are used or implied, and this project is not affiliated with or endorsed by Swish.
