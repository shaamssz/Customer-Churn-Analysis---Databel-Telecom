# Customer Churn Analysis — Databel Telecom

> *A business case study examining what is driving 26.9% customer churn at a US telecom provider — and what the retention team should do about it.*

---

## Table of Contents

1. [Business Context & Problem Statement](#1-business-context--problem-statement)
2. [Objectives](#2-objectives)
3. [Project Scope & Tools](#3-project-scope--tools)
4. [Repository Structure](#4-repository-structure)
5. [Data Workflow](#5-data-workflow)
6. [Data Model & Schema](#6-data-model--schema)
7. [Analysis & Metrics](#7-analysis--metrics)
8. [Key Insights](#8-key-insights)
9. [Recommendations](#9-recommendations)
10. [Assumptions & Limitations](#10-assumptions--limitations)
11. [Future Enhancements](#11-future-enhancements)
12. [Author](#12-author)

---

## 1. Business Context & Problem Statement

### The Industry Problem

Customer churn is one of the most damaging and expensive problems in the telecom industry. Acquiring a new customer costs significantly more than retaining an existing one, yet most telecom providers struggle to identify *who* is at risk of leaving, *why* they leave, and *when* to intervene — before it is too late.

For a mid-sized provider like Databel, operating across all 50 US states in a saturated market where switching costs are low and competitors are aggressive, even a few percentage points of avoidable churn translates directly into lost recurring revenue and wasted acquisition spend.

### The Question

With an overall churn rate sitting at **26.9%** — well above the ~15–20% typical for US telecom — the central question this analysis set out to answer was not simply *how many* customers are leaving, but *which ones*, *under what conditions*, and *what that tells us about where the business should focus*.

Specifically:

1. **How bad is it?** What is the actual churn rate across the customer base, and where does it concentrate?
2. **Who is leaving?** Which segments — by contract type, age group, geography, and plan type — show the highest churn rates?
3. **What is driving it?** Which product, pricing, or service factors appear most strongly associated with the decision to cancel?

### The Approach & Outcome

Using Databel's customer dataset (~6,700 records), churn was analyzed across multiple dimensions: contract type, demographics, international plan usage, data consumption behavior, and state-level geography. The analysis identified three high-priority segments — month-to-month contract holders, international plan subscribers in specific states, and senior customers (65+) — and produced a set of targeted, prioritized retention recommendations grounded directly in the data.

---

## 2. Objectives

- **Primary Objective:** Calculate the overall churn rate and identify which customer segments churn at the highest rates.
- **Secondary Objective 1:** Determine whether contract type (month-to-month vs. one-year vs. two-year) is a meaningful predictor of churn.
- **Secondary Objective 2:** Assess whether international plan usage and extra international charges correlate with elevated churn rates.
- **Secondary Objective 3:** Explore whether demographic factors (age group, senior status) or data consumption patterns differ meaningfully across churned and retained customers.
---

## 3. Project Scope & Tools

### Scope

| Dimension | Details |
|---|---|
| **In Scope** | All customers in the Databel aggregate dataset across 50 US states; churn by contract type, demographics, data usage, and international plan |
| **Out of Scope** | Individual-level predictive modeling; time-series churn trends (no time dimension in this dataset) |
| **Time Period** | Static snapshot — no date range; dataset represents a single period |
| **Granularity** | Aggregated customer-level rows (one row per unique combination of attributes); not raw transactional data |

### Tools & Technologies

| Category | Tool(s) Used |
|---|---|
| Data Storage | CSV / Excel (.xlsx) |
| Data Processing | Microsoft Excel — calculated columns, structured tables |
| Analysis | Excel PivotTables, GETPIVOTDATA formulas |
| Visualization | Excel PivotCharts, dashboard sheet |
| Version Control | Git / GitHub |
| Documentation | Markdown |

---

## 4. Repository Structure

```
customer-churn-analysis/
│
├── data/
│   ├── raw/                  # 1_1_data_preparation.xlsx — original unmodified source
│   └── processed/            # Customer_Churn_Analysis_.xlsx — cleaned and enriched workbook
│
├── visuals/                  # Exported charts and dashboard 
│
└── README.md                 # You are here
```

> ⚠️ *The raw file (`raw_dataset.xlsx`) is kept unmodified as the source of truth. All cleaning, enrichment, and analysis was performed in the separate processed workbook.*

---

## 5. Data Workflow

```
[Databel Aggregate Dataset — Excel]
            ↓
[Data Cleaning & Column Enrichment in Excel]
            ↓
[PivotTable Construction across key dimensions]
            ↓
[Calculated Metrics (Churn Rate %, grouped fields)]
            ↓
[Overview Dashboard — summary KPIs + state-level churn table]
```

1. **Source:** Pre-aggregated customer data provided as an Excel workbook. Each row represents a unique combination of customer attributes (state, age group, contract type, plan type, etc.) with counts of total and churned customers.
2. **Ingestion:** Opened directly in Excel; no external connection or import required.
3. **Cleaning:** Added a `Demographics` grouping column (Under 30 / Senior / Other); added `Grouped Consumption` field bucketing average monthly GB download into `Less than 5 GB`, `Between 5 and 10 GB`, and `10 or more GB`.
4. **Transformation:** Created PivotTables on the `Customer Pivots` sheet to aggregate churn counts and calculate churn rates across multiple dimensions. Used `GETPIVOTDATA` to pull headline KPIs into the Overview dashboard.
5. **Analysis:** Compared churn rates by contract type, age group, international plan status, state, data consumption tier, and unlimited plan subscription. Cross-tabulated churn rate against unlimited plan flag and consumption bucket.
6. **Output:** Summary dashboard (`Overview` sheet) with headline KPIs and a ranked state-level churn table for international plan subscribers. Detailed pivot analysis on `Churn Analysis` and `Customer Pivots` sheets.

---

## 6. Data Model & Schema

### Dataset: `Databel - Customer` (Raw)

| Field Name | Data Type | Description | Example Value |
|---|---|---|---|
| `Customer ID` | String | Unique customer identifier | `4444-BZPU` |
| `Churn Label` | String | Whether the customer churned | `Yes` / `No` |
| `Account Length (in months)` | Integer | Tenure of the customer | `33` |
| `Contract Type` | String | Subscription contract length | `Month-to-Month` |
| `Payment Method` | String | How the customer pays | `Direct Debit` |
| `Monthly Charge` | Float | Monthly bill amount (USD) | `21` |
| `Total Charges` | Float | Cumulative charges (USD) | `703` |
| `Intl Plan` | String | Whether customer has international plan | `yes` / `no` |
| `Intl Active` | String | Whether customer actively used international calls | `Yes` / `No` |
| `Extra International Charges` | Float | Overage charges for international usage (USD) | `6.30` |
| `Avg Monthly GB Download` | Float | Average monthly data consumption (GB) | `10` |
| `Unlimited Data Plan` | String | Whether the customer has an unlimited data plan | `Yes` / `No` |
| `Extra Data Charges` | Float | Overage charges for data usage (USD) | `5` |
| `Customer Service Calls` | Integer | Number of calls made to customer service | `2` |
| `State` | String | US state abbreviation | `CA` |
| `Gender` | String | Customer gender | `Female` |
| `Age` | Integer | Customer age | `35` |
| `Under 30` | String | Age group flag | `Yes` / `No` |
| `Senior` | String | Senior status flag (65+) | `Yes` / `No` |
| `Churn Category` | String | High-level reason for churn | `Competitor` |
| `Churn Reason` | String | Specific reason for churn | `Competitor made better offer` |

> **Row count (approx.):** ~6,700 customer records

> **Key relationship:** The aggregate sheet groups individual customers by shared attribute combinations; `Total Customers` and `Churned Customers` columns represent counts per group.

### Enriched Fields (Added During Analysis)

| Field Name | Description |
|---|---|
| `Demographics` | Derived grouping: `Under 30`, `Senior`, or `Other` |
| `Grouped Consumption` | Bucketed data usage: `Less than 5 GB`, `Between 5 and 10 GB`, `10 or more GB` |

---

## 7. Analysis & Metrics

### Analytical Approach

This is an exploratory analysis — no hypothesis was tested formally. The goal was to surface patterns in churn rates across multiple customer dimensions and identify where the highest-risk segments concentrate. PivotTables were used to slice churn rate by each dimension independently, and a cross-tabulation was performed to examine the interaction between unlimited plan status and data consumption tier.

### Key Metrics Defined

| Metric | Plain-Language Definition | Why It Matters |
|---|---|---|
| `Churn Rate %` | Churned Customers ÷ Total Customers | Core KPI — the proportion of customers who left |
| `Avg Monthly Charges` | Average monthly bill per customer group | Higher charges may correlate with churn risk |
| `Avg Customer Service Calls` | Average number of support contacts per customer group | Proxy for dissatisfaction or service friction |
| `Avg Extra International Charges` | Average overage charges for international usage | High unexpected charges are a known churn driver |

### Methods Used

- Descriptive statistics: overall churn rate, average charges, average service call volume
- Segmentation by: contract type, age group (Under 30 / Senior / Other), state, international plan status, data consumption tier, unlimited plan flag
- Cross-tabulation: unlimited plan × data consumption bucket × churn rate
- State-level ranking of churn rates for international plan subscribers

---

## 8. Key Insights

**Insight 1: Overall churn rate is 26.9% — well above typical industry benchmarks.**
Out of 6,687 customers, 1,796 churned. At nearly 1 in 4 customers, this level of attrition represents a significant and urgent business problem.

**Insight 2: Month-to-month contracts are by far the highest churn risk.**
Customers on month-to-month contracts churn at ~74%, compared to ~12% on one-year and materially lower rates on two-year contracts. Contract type is the single strongest differentiator in this dataset.

**Insight 3: Senior customers churn at a meaningfully higher rate (~38%) than other age groups.**
Customers aged 65 and above churn at roughly 14 percentage points above the overall average. This could reflect service complexity, competitor pricing targeting older demographics, or support experience issues.

**Insight 4: International plan subscribers in certain states show extreme churn rates.**
California leads at 75% churn among international plan users, followed by Indiana (67%) and New Hampshire (63%). Many of these customers appear to have the plan activated but may not be actively using international minutes — suggesting they are paying for a feature they don't need or were mis-sold.

**Insight 5: Customers without an unlimited data plan who consume 10+ GB per month churn at elevated rates.**
The cross-tabulation reveals that high-consumption customers without unlimited plans face extra data charges, which likely drives dissatisfaction. Customers on unlimited plans show relatively stable churn regardless of consumption tier.

---

## 9. Recommendations

| Priority | Recommendation | Based On | Suggested Owner |
|---|---|---|---|
| High | Launch a contract upgrade incentive — offer month-to-month customers a discount to switch to a one-year contract | Insight 2: 74% churn on M2M contracts | Retention / Commercial team |
| High | Audit international plan subscriptions — proactively reach out to customers with the plan but low international usage | Insight 4: High churn in states with low Intl Active rates | Customer Success / CRM team |
| Medium | Design a senior-specific retention program — simplify billing, offer dedicated support, or create loyalty pricing | Insight 3: 38% senior churn rate | Marketing / Product team |
| Medium | Offer unlimited plan upgrades to high-data customers before they incur overage charges | Insight 5: High churn among heavy data users on non-unlimited plans | Billing / Product team |
| Low | Investigate the specific churn reasons in the top-churning states (CA, IN, NH) for international plan users to determine whether mis-selling is occurring | Insight 4: Extreme state-level churn rates | Compliance / Sales Quality team |

---

## 10. Assumptions & Limitations

### Assumptions

- The `Churned Customers` field accurately reflects customers who have left the service — the data is treated as ground truth without independent verification.
- The aggregate-level dataset is assumed to faithfully represent the underlying individual customer population.
- Demographics groupings (Under 30, Senior) use the `Age` field as-is, assuming ages are self-reported and accurate.
- "Senior" is defined as age 65+ based on the dataset's `Senior` flag.

### Limitations

- **No time dimension:** The dataset is a static snapshot. Churn trends over time cannot be assessed — it is unknown whether churn is improving, worsening, or seasonal.
- **Aggregated data:** Rows represent groups of customers sharing the same attribute combination, not individual records. This limits granularity — for example, you cannot analyze the full distribution of charges within a segment.
- **No customer lifetime value:** Revenue impact of churn cannot be calculated without total charges or tenure-based LTV data.
- **Causation vs. correlation:** High churn rates in segments (e.g., month-to-month contracts, senior customers) are correlational, not causal. Customers who were already planning to leave may have switched to shorter contracts beforehand — a potential reverse causality issue.
- **Limited churn reason coverage:** The `Churn Category` and `Churn Reason` fields are only populated for churned customers, and not all churned rows have a recorded reason.

---

## 11. Future Enhancements

- [ ] Rebuild analysis on individual-level (non-aggregated) data to enable distribution analysis and regression modeling
- [ ] Add a time dimension to track monthly churn rates and identify seasonal patterns
- [ ] Build a logistic regression or decision tree model to predict individual churn probability
- [ ] Incorporate customer lifetime value (LTV) to prioritize which churned segments cost the business the most
- [ ] Create an interactive dashboard in Tableau or Power BI for stakeholder-facing reporting
- [ ] Investigate whether customers who downgraded from long-term to month-to-month contracts before churning represent a leading indicator

---

## 12. Author

**Abdulaziz** - 
Data Analytics Portfolio Project

- 🔗 https://www.linkedin.com/in/abdulaziz-abidzhanov-62656b343/
- 💼 https://github.com/shaamssz


---

*Last updated: May 2026*
