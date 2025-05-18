# DataAnalytics-Assessment

# SQL Assessment Solutions

This repository contains SQL solutions to a set of assessment tasks that are focused on user behavior analysis, product usage, and customer value estimation. The queries leverage common SQL techniques like aggregation, joins, filtering, and CTEs to draw meaningful insights from transactional data.

---

## Question 1: High-Value Customers with Multiple Products

**Objective:**  
Identify customers who own both a savings and an investment plan, and rank them by total confirmed deposits.

**Approach:**  
- Used subqueries to count savings and investment plans per user.
- Joined counts back to the users table.
- Pulled in total deposits (from savings accounts) via a left join.
- Converted deposit amounts from kobo to full currency units.
- Sorted results in descending order of deposit volume.

**Challenges:**  
- Ensuring correct user linkage across savings and investment subqueries.
- Handling users with no deposits using `IFNULL`.
- Making sure the currency conversion and rounding were precise.

---

## Question 2: Transaction Frequency Analysis

**Objective:**  
Classify users by how frequently they transact on a monthly basis.

**Approach:**  
- Counted the number of inflow transactions per user, per month.
- Calculated the average number of monthly transactions per user.
- Assigned users into frequency categories (High, Medium, Low).
- Aggregated and displayed the number of users per category, with average monthly transaction volumes.

**Challenges:**  
- Excluding null or zero-value transactions to avoid skewing averages.
- Ensuring the logic for frequency classification was clean and consistent.
- Maintaining meaningful sort order with `FIELD()` for better readability.

---

## Question 3: Account Inactivity Alert

**Objective:**  
Flag accounts with no confirmed transactions in the past 365 days.

**Approach:**  
- Isolated all active plans (non-deleted and with an active status).
- Joined transactions, ensuring only confirmed amounts were considered.
- Pulled the most recent transaction date for each plan.
- Used `COALESCE` to handle null dates and compute accurate inactivity durations.
- Grouped and labeled each plan as either Savings, Investment, or Other.

**Challenges:**  
- Avoiding false positives due to missing transaction data.
- Ensuring accurate date computations using `DATEDIFF()` and `MAX()`.
- Getting clean output even for accounts with no activity at all.

---

## Question 4: Customer Lifetime Value (CLV) Estimation

**Objective:**  
Estimate CLV for each customer, assuming a fixed profit rate of 0.1% per transaction.

**Approach:**  
- Calculated account tenure in months from the signup date.
- Counted total confirmed transactions and summed up confirmed transaction amounts.
- Estimated CLV using a simplified formula:  
  ```
  CLV = (Total Transactions / Tenure) * 12 * Avg Profit Per Transaction
  ```
- Ranked users by CLV in descending order.

**Challenges:**  
- Prevented division by zero when tenure is 0 using `NULLIF`.
- Balanced aggregation logic while preserving user-level granularity.
- Ensured transactions linked to a valid plan to avoid noise.

---

## Query Tool

- **MySQL
- **Key Tables Used:**
  - `users_customuser`
  - `plans_plan`
  - `savings_savingsaccount`

---

## SQL Files Attached

| File              | Description |
|-------------------|-------------|
| `Assessment_Q1.sql` | Multi-product with high-value customer analysis |
| `Assessment_Q2.sql` | Monthly transaction frequency |
| `Assessment_Q3.sql` | Detection of inactive accounts |
| `Assessment_Q4.sql` | Customer Lifetime Value (CLV) estimation |

---

## Notes

- All queried tables are based on the provided database.
- NULL and 0-value edge cases were considered where necessary.
- Readability and logical structure were prioritized.
