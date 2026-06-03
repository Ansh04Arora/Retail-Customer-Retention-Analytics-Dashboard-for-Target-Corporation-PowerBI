# Retail-Customer-Retention-Analytics-Dashboard-for-Target-Corporation-PowerBI

## Overview

This project focuses on analyzing customer retention, loyalty engagement, purchasing behavior, and customer lifetime value for Target Corporation using Microsoft Power BI.

The objective was to transform raw customer, transaction, loyalty, store, and churn data into an interactive analytics dashboard that helps identify retention opportunities and support data-driven business decisions.

---

## Problem Statement

Target operates across multiple retail formats including physical stores and online channels. With increasing competition from major retailers, understanding customer retention has become critical.

The project addresses the following business questions:

- Which customers are most likely to churn?
- Which customer segments generate the highest value?
- How effective are promotions in influencing purchases?
- How well is the loyalty program performing?
- Which store types contribute most to customer retention?
- How can Target improve long-term customer engagement?

---

## Dataset Information

The project uses five datasets:

### Customer_Demographics
Contains:
- Customer ID
- Age
- Gender
- Region
- Income Level
- Membership Date
- Preferred Channel

### Customer_Transactions
Contains:
- Transaction ID
- Customer ID
- Store ID
- Product Category
- Transaction Date
- Transaction Amount
- Promotion Applied

### Store_Locations
Contains:
- Store ID
- Store Type
- Region
- Opening Year

### Loyalty_Program
Contains:
- Customer ID
- Loyalty Tier
- Points Earned
- Points Redeemed

### Churn_Labelled_Customers
Contains:
- Customer ID
- Last Purchase Date
- Churn Flag
- Churn Reason

---

## Project Workflow

### Task 1 – Data Preparation & Modeling

- Imported datasets into Power BI
- Cleaned missing values
- Removed duplicate records
- Corrected data types
- Created calculated columns
- Built star schema data model

#### Calculated Columns

```DAX
Membership_Duration_Years =
Duration.Days(Date.From(DateTime.LocalNow()) - [Membership_Since]) / 365
```

---

### Task 2 – Churn & Retention Analysis

#### Measures

```DAX
Total Customers =
DISTINCTCOUNT(Customer_Demographics[Customer_ID])
```

```DAX
Churned Customers =
CALCULATE(
DISTINCTCOUNT(Churn_Labelled_Customers[Customer_ID]),
Churn_Labelled_Customers[Churn_Flag] = 1
)
```

```DAX
Churn Rate =
DIVIDE(
[Churned Customers],
[Total Customers],
0
)
```

#### Analysis Performed

- Overall Churn Rate
- Churn by Region
- Churn by Income Group
- Churn by Channel
- Churn by Loyalty Tier
- Customer Funnel Analysis

---

### Task 3 – Repeat Purchase Analysis

#### Calculated Columns

```DAX
Purchase Count =
CALCULATE(
COUNT(Customer_Transactions[Transaction_ID]),
RELATEDTABLE(Customer_Transactions)
)
```

```DAX
Purchase Tier =
SWITCH(
TRUE(),
[Purchase Count] <= 3,"Low-Tier",
[Purchase Count] <= 8,"Mid-Tier",
"High-Tier"
)
```

#### Analysis Performed

- Customer Segmentation
- Purchase Frequency by Region
- Purchase Frequency by Age Group
- Purchase Frequency by Loyalty Tier
- Product Categories Purchased by Loyal Customers

---

### Task 4 – Promotion & Loyalty Impact

#### Measures

```DAX
Promotion Transactions =
CALCULATE(
COUNTROWS(Customer_Transactions),
Customer_Transactions[Promotion_Applied] = "Yes"
)
```

```DAX
Promotion % =
DIVIDE(
[Promotion Transactions],
COUNTROWS(Customer_Transactions),
0
)
```

```DAX
Avg Amount With Promotion =
CALCULATE(
AVERAGE(Customer_Transactions[Amount]),
Customer_Transactions[Promotion_Applied] = "Yes"
)
```

```DAX
Avg Amount Without Promotion =
CALCULATE(
AVERAGE(Customer_Transactions[Amount]),
Customer_Transactions[Promotion_Applied] = "No"
)
```

#### Analysis Performed

- Promotion Usage Rate
- Promotion Impact on Purchase Amount
- Loyalty Tier Churn Analysis
- Points Earned vs Redeemed

---

### Task 5 – Store & Channel Performance

#### Measures

```DAX
Avg Transaction Amount =
AVERAGE(Customer_Transactions[Amount])
```

```DAX
Retention Rate =
1 - [Churn Rate]
```

#### Analysis Performed

- Average Transaction Amount by Store Type
- Retention Rate by Store Type
- Customer Distribution by Store Type
- Store Opening Year vs Retention

---

### Task 6 – Customer Lifetime Value (CLV)

#### Measures

```DAX
Total Amount Spent =
CALCULATE(
SUM(Customer_Transactions[Amount]),
RELATEDTABLE(Customer_Transactions)
)
```

```DAX
CLV =
DIVIDE(
[Total Amount Spent],
[Membership_Duration_Years],
0
)
```

```DAX
Average CLV =
AVERAGE(Customer_Demographics[CLV])
```

#### Analysis Performed

- CLV Segmentation
- CLV by Region
- CLV by Loyalty Tier
- CLV vs Days Since Last Purchase

---

## Key Findings

- Overall churn rate was 24.5%.
- Mid-Tier customers represented the largest customer segment.
- High-Tier customers accounted for a small percentage of the customer base.
- Apparel emerged as the most purchased category among loyal customers.
- Promotions showed limited impact on increasing transaction value.
- Loyalty points earned significantly exceeded points redeemed.
- High-CLV customers contributed the highest long-term value.

---

## Recommendations

### 1. Improve Loyalty Point Redemption

Introduce reward reminders and simplified redemption processes to increase customer engagement.

### 2. Convert Mid-Tier Customers into High-Tier Customers

Use personalized promotions and loyalty incentives to encourage repeat purchases.

### 3. Prioritize High-Value Customers

Target high CLV customers with retention campaigns to reduce revenue loss from potential churn.

---

## Tools & Technologies

- Microsoft Power BI
- Power Query
- DAX (Data Analysis Expressions)
- Data Modeling
- Data Visualization

---

## Skills Demonstrated

- Data Cleaning
- Data Modeling
- DAX Calculations
- Customer Segmentation
- Churn Analysis
- Customer Lifetime Value Analysis
- Dashboard Design
- Business Intelligence Reporting

---

## Author

**Ansh Arora**

Power BI | Data Analytics | Business Intelligence
