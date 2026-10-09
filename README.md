# UPI Fraud & Transaction Risk Analytics Dashboard

A 5-page Power BI dashboard that analyses UPI payment fraud patterns across 2022 to 2025: which transaction types, payees, hours and customer groups carry the most risk, and what a bank could do about it.

![Overview](screenshots/01_overview.png)

## Problem statement
UPI volumes and fraud losses have both grown sharply. A bank's risk team needs to know where fraud concentrates so it can add friction where it matters without slowing genuine payments. This project answers four questions:

1. How big is the fraud problem and how is it trending?
2. What kinds of fraud happen, and how costly are they?
3. When, where and on which merchant categories does fraud cluster?
4. Which customer behaviours and segments are riskiest?

## Data
| File | What it is |
|---|---|
| `data/upi_transactions_2022_2025_clean_5k.csv` | 5,000 transaction records, Jan 2022 to Dec 2025 (17 columns) |
| `data/upi_fraud_national_yearly_real_anchor.csv` | Official national UPI fraud figures used as context |

**Important: the transaction data is simulated.** Real transaction-level UPI fraud data is not public. The sample was generated in Python and calibrated to public patterns (month-wise growth, rising merchant-payment share, and the year-to-year shape of fraud against volume).

**Fraud is deliberately oversampled** (6% of rows versus a far lower rate in the real system) so that every fraud type has enough rows to analyse. Rates in this dashboard are therefore **sample rates, not national rates**.

### Real national context (Lok Sabha replies, Ministry of Finance)
| Financial year | Fraud incidents | Fraud loss |
|---|---|---|
| FY2021-22 | not used | Rs 242 crore |
| FY2022-23 | about 7.25 lakh | Rs 573 crore |
| FY2023-24 | about 13.42 lakh | Rs 1,087 crore |
| FY2024-25 | about 12.64 lakh | Rs 981 crore |
| FY2025-26 (Apr to Nov only) | over 10.64 lakh | Rs 805 crore |

The simulated sample was built so its fraud rate rises to a peak in 2023 and then falls, following the shape of the real figures. This is a calibration choice, not independent validation.

## Dashboard pages
| Page | Question it answers |
|---|---|
| 1. Overview | Transactions, value, fraud cases, fraud rate and the quarterly trend |
| 2. Fraud landscape | Fraud type, value, amount bucket and how fast fraud is reported |
| 3. When and where | Hour by weekday heatmap, state map, merchant categories |
| 4. Risk profile | New versus known payees, account age, velocity, age group, highest-risk segments |
| 5. Insights and recommendations | Findings, four recommendations and fraud rate by year |

Screenshots: `screenshots/01_overview.png`, `02_fraud_landscape.png`, `03_when_and_where.png`, `04_risk_profile.png`, `05_insights.png`.

## Key findings (sample data)
- **Collect requests are the riskiest transaction type**: about 32% fraud rate versus about 6% overall.
- **New payees carry about 5.6 times the fraud rate of known payees** (17.7% versus 3.2%).
- **Night hours (10pm to 4am) show about 3 times the fraud rate of daytime** (15.5% versus 5.2%).
- Collect requests to new payees are the highest-risk segment: 29% by day (230 transactions) and 70% at night (only 30 transactions, so directional).
- Age groups 60+ and 18-25 show the highest rates (8.6% and 7.7%).
- Fraud rate by year in the sample: 6.7% (2022), 9.2% (2023), 5.2% (2024), 4.9% (2025).

## Recommendations
1. Add a confirmation step for collect requests from first-time payees.
2. Apply step-up authentication to night-time transfers to new payees.
3. Limit or delay high-value payments from accounts under 30 days old.
4. Show scam warnings to users aged 60+ and 18-25.

## Method
- **Power Query:** data types and load checks.
- **DAX measures:** Total Transactions, Successful Transactions, Fraud Cases, Fraud Value, Fraud Rate % (fraud cases divided by successful transactions), Avg Fraud Ticket, Median Report Delay, segment rate with a minimum-volume rule, and dynamic insight text.
- **Calculated columns:** hour, weekday, amount bucket, account age bucket, velocity bucket, payee status, risk segment.
- **Report design:** 1920 x 1080 canvas, page navigator, slicers, cross-filtering and a consistent colour system (teal for normal, red for fraud).

## Limitations
- Transactions are simulated, so findings describe the sample, not real customers.
- Fraud is oversampled, so rates are not national rates.
- State, account-age and velocity results rest on small counts. Treat them as directions, not precise numbers. Velocity is mixed with transaction type and shows no clean pattern.
- Differences by merchant category and by month are small and partly driven by transaction mix, so they are not used for recommendations.

## How to open
1. Install Power BI Desktop (Windows).
2. Open `UPI_Fraud_Risk_Analytics.pbix`. The data is embedded in the file.
3. To refresh from the CSV, update the file path in Power Query (Transform data, Source).

## Tools
Power BI, DAX, Power Query, Python (data simulation)
