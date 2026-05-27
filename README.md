# Customer Churn Analysis: Databel Telecom

> A business case study looking at why 26.9% of Databel's customers are leaving, and where the retention team should focus first.

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

Churn is an expensive problem for any telecom company. Winning a new customer costs a lot more than keeping one you already have, and most providers have a hard time spotting who is about to leave until they have already gone.

Databel is a mid-sized provider operating across all 50 US states. The market is crowded, switching costs are low, and competitors are quick to undercut on price. In that kind of environment, even a small share of avoidable churn adds up to real money lost, both in recurring revenue and in the cost of replacing those customers.

### The Question

Databel's churn rate sits at 26.9%, well above the 15 to 20% that is normal for US telecom. The point of this analysis was less about the headline number and more about what sits underneath it: which customers are leaving, what they have in common, and where the business can actually do something about it.

Three questions guided the work:

1. How bad is it, and where does the churn concentrate?
2. Who is leaving, broken down by contract type, age, geography, and plan?
3. What seems to be driving the decision to cancel?

### The Approach & Outcome

I worked through Databel's customer dataset of roughly 6,700 records and looked at churn across contract type, demographics, international plan usage, data consumption, and state. Three groups stood out as the clearest priorities: month-to-month customers, international plan subscribers in a handful of states, and customers aged 65 and over. The recommendations at the end of this document come straight out of those patterns.

---

## 2. Objectives

- **Primary Objective:** Calculate the overall churn rate and find which customer segments churn the most.
- **Secondary Objective 1:** Check whether contract type (month-to-month, one-year, two-year) predicts churn.
- **Secondary Objective 2:** See whether international plan usage and extra international charges line up with higher churn.
- **Secondary Objective 3:** Look at whether age group, senior status, or data consumption patterns differ between customers who left and those who stayed.

---

## 3. Project Scope & Tools

### Scope

| Dimension | Details |
|---|---|
| **In Scope** | All customers in the Databel dataset across 50 US states; churn by contract type, demographics, data usage, and international plan |
| **Out of Scope** | Individual-level predictive modeling; time-series churn trends (the data has no time dimension) |
| **Time Period** | A single static snapshot with no date range |
| **Granularity** | Aggregated customer-level rows, one row per unique combination of attributes |

### Tools & Technologies

| Category | Tool(s) Used |
|---|---|
| Data Storage | CSV / Excel (.xlsx) |
| Data Processing | Microsoft Excel (calculated columns, structured tables) |
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
│   ├── raw/                  # 1_1_data_preparation.xlsx (original source file)
│   └── processed/            # Customer_Churn_Analysis_.xlsx (cleaned and analyzed workbook)
│
├── visuals/                  # Exported charts and dashboard screenshots
│
└── README.md                 # You are here
```

> The raw file stays untouched as the source of truth. All cleaning and analysis happened in the processed workbook.

---

## 5. Data Workflow

```
[Databel Dataset (Excel)]
            ↓
[Cleaning & Column Enrichment in Excel]
            ↓
[PivotTables across key dimensions]
            ↓
[Calculated Metrics (Churn Rate %, grouped fields)]
            ↓
[Overview Dashboard: KPIs + state-level churn table]
```

1. **Source:** Pre-aggregated customer data in an Excel workbook. Each row is a unique combination of customer attributes (state, age group, contract type, plan type) with counts of total and churned customers.
2. **Ingestion:** Opened directly in Excel. No external connection or import was needed.
3. **Cleaning:** Added a `Demographics` column grouping customers into Under 30, Senior, or Other. Added a `Grouped Consumption` field that buckets average monthly GB download into `Less than 5 GB`, `Between 5 and 10 GB`, and `10 or more GB`.
4. **Transformation:** Built PivotTables on the `Customer Pivots` sheet to total up churn counts and work out churn rates across each dimension. Used `GETPIVOTDATA` to pull the headline KPIs into the Overview dashboard.
5. **Analysis:** Compared churn rates by contract type, age group, international plan status, state, data consumption tier, and unlimited plan status. Cross-tabulated churn against the unlimited plan flag and consumption bucket.
6. **Output:** A summary dashboard on the `Overview` sheet with the headline KPIs and a ranked state-level churn table for international plan subscribers. The detailed pivots live on the `Churn Analysis` and `Customer Pivots` sheets.

---

## 6. Data Model & Schema

### Dataset: `Databel - Customer` (Raw)

| Field Name | Data Type | Description | Example Value |
|---|---|---|---|
| `Customer ID` | String | Unique customer identifier | `4444-BZPU` |
| `Churn Label` | String | Whether the customer churned | `Yes` / `No` |
| `Account Length (in months)` | Integer | Customer tenure | `33` |
| `Contract Type` | String | Subscription contract length | `Month-to-Month` |
| `Payment Method` | String | How the customer pays | `Direct Debit` |
| `Monthly Charge` | Float | Monthly bill amount (USD) | `21` |
| `Total Charges` | Float | Cumulative charges (USD) | `703` |
| `Intl Plan` | String | Whether the customer has an international plan | `yes` / `no` |
| `Intl Active` | String | Whether the customer actively used international calls | `Yes` / `No` |
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

> **Key relationship:** The aggregate sheet groups customers by shared attribute combinations. The `Total Customers` and `Churned Customers` columns hold the counts per group.

### Enriched Fields (Added During Analysis)

| Field Name | Description |
|---|---|
| `Demographics` | Grouping into `Under 30`, `Senior`, or `Other` |
| `Grouped Consumption` | Data usage bucket: `Less than 5 GB`, `Between 5 and 10 GB`, `10 or more GB` |

---

## 7. Analysis & Metrics

### Analytical Approach

This was an exploratory analysis with no formal hypothesis test. The aim was to find where churn concentrates across customer dimensions. I used PivotTables to slice churn rate by each dimension on its own, then ran a cross-tab to see how unlimited plan status and data consumption interact.

### Key Metrics Defined

| Metric | Plain-Language Definition | Why It Matters |
|---|---|---|
| `Churn Rate %` | Churned Customers ÷ Total Customers | The core KPI: the share of customers who left |
| `Avg Monthly Charges` | Average monthly bill per customer group | Higher charges may line up with churn risk |
| `Avg Customer Service Calls` | Average support contacts per customer group | A proxy for frustration or service friction |
| `Avg Extra International Charges` | Average international overage charges | Surprise charges are a known churn driver |

### Methods Used

- Descriptive statistics: overall churn rate, average charges, average service call volume
- Segmentation by contract type, age group, state, international plan status, data consumption tier, and unlimited plan flag
- A cross-tab of unlimited plan status against data consumption bucket and churn rate
- State-level ranking of churn rates for international plan subscribers

---

## 8. Key Insights

**Insight 1: The overall churn rate is 26.9%, well above industry norms.**
Of 6,687 customers, 1,796 left. Almost 1 in 4 customers churning is a serious problem and the main reason this analysis matters.

**Insight 2: Month-to-month contracts churn far more than any other group.**
Month-to-month customers churn at about 74%, compared with about 12% on one-year contracts and even less on two-year contracts. Contract type is the single strongest signal in the data.

**Insight 3: Senior customers churn at about 38%.**
Customers aged 65 and over churn roughly 14 points above the overall average. This could come down to billing complexity, competitor pricing aimed at older customers, or a support experience that does not work for them.

**Insight 4: International plan subscribers in a few states churn at extreme rates.**
California tops the list at 75% churn among international plan users, followed by Indiana at 67% and New Hampshire at 63%. A lot of these customers have the plan switched on but show low actual international usage, which suggests they are paying for something they do not use or were signed up for it without needing it.

**Insight 5: Heavy data users without an unlimited plan churn at higher rates.**
The cross-tab shows that customers downloading 10 or more GB a month without an unlimited plan rack up extra data charges, and they leave more often. Customers on unlimited plans churn at a steady rate no matter how much data they use.

---

## 9. Recommendations

**1. Get month-to-month customers onto longer contracts.**
This is where the churn problem really lives, so it is the first place to act. Offer month-to-month customers a discount or a perk to move to a one-year contract. Even a modest shift here would cut the biggest source of churn. Start with the customers who have been on month-to-month the longest, since they are the most likely to walk. *Owner: Retention / Commercial.*

**2. Review who actually needs the international plan.**
A lot of international plan subscribers in the high-churn states barely make international calls. Pull the list of customers who have the plan but show low usage, and have the team reach out to either right-size their plan or move them to one that fits how they actually use the service. This is a problem the customer feels on every single bill, so fixing it should pay off quickly. *Owner: Customer Success / CRM.*

**3. Build a retention play aimed at customers 65 and over.**
Senior customers leave at a noticeably higher rate, and a generic offer will not fix that. Test a few things with this group: simpler billing, a dedicated support line, or loyalty pricing. Run it as a small pilot first and measure whether churn in the group drops before rolling it out. *Owner: Marketing / Product.*

**4. Move heavy data users onto unlimited plans before the overage charges hit.**
Customers downloading 10 or more GB without an unlimited plan are paying surprise overage fees, and they leave more often because of it. Flag these accounts and offer the upgrade before the next bill goes out. For the customer it is a better deal, and for Databel it removes an obvious reason to shop around. *Owner: Billing / Product.*

**5. Find out why California, Indiana, and New Hampshire are so bad.**
The state-level numbers are extreme enough that something specific is going on, possibly a sales practice that signs people up for plans they do not need. Dig into the churn reasons for international plan users in these three states before spending money on a fix, so the fix actually targets the cause. *Owner: Sales Quality / Compliance.*

---

## 10. Assumptions & Limitations

### Assumptions

- The churn field is treated as correct. I did not independently verify whether each flagged customer actually left.
- The aggregated dataset is taken as a fair representation of the underlying customer population.
- Age-based groups use the `Age` field as it stands, assuming the ages are accurate.
- "Senior" means age 65 and over, based on the dataset's own `Senior` flag.

### Limitations

- **No time dimension.** The data is a single snapshot, so there is no way to tell whether churn is getting better, worse, or moving with the seasons.
- **Aggregated data.** Rows represent groups of customers who share the same attributes, not individuals. That limits how deep the analysis can go. You cannot see the full spread of charges inside a segment, for example.
- **No lifetime value.** Without tenure-based LTV data, I could not put a dollar figure on what each churned segment costs the business.
- **Correlation, not causation.** The high churn rates by segment show association, not cause. Month-to-month customers might already have been on their way out before they picked that contract, which would flip the direction of the relationship.
- **Patchy churn reasons.** The `Churn Category` and `Churn Reason` fields are only filled in for customers who left, and not even all of those have a reason recorded.

---

## 11. Future Enhancements

- [ ] Rerun the analysis on individual-level data to allow distribution analysis and modeling
- [ ] Add a time dimension to track monthly churn and catch seasonal patterns
- [ ] Build a logistic regression or decision tree to predict churn at the customer level
- [ ] Bring in customer lifetime value to rank segments by what their churn actually costs
- [ ] Rebuild the dashboard in Tableau or Power BI for a stakeholder-facing version
- [ ] Check whether customers who downgrade to month-to-month before leaving act as an early warning sign

---

## 12. Author

**Abdulaziz** - 
Data Analytics Portfolio Project

- 🔗 https://www.linkedin.com/in/abdulaziz-abidzhanov-62656b343/
- 💼 https://github.com/shaamssz


---

*Last updated: May 2026*
