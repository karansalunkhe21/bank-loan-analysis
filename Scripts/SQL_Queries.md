# Bank Loan Analysis – SQL Queries Documentation

This document contains SQL queries used to validate and support metrics displayed in the **Bank Loan Analysis Power BI Dashboard**.

---

## Key Metrics

### Total Loan Applications
```sql
SELECT COUNT(id) AS Total_Applications
FROM bank_loan_data;

```
## MTD Loan Applications
```sql
SELECT COUNT(id) AS Total_Applications
FROM bank_loan_data
WHERE MONTH(issue_date) = 12;

```
## PMTD Loan Applications
```sql
SELECT COUNT(id) AS Total_Applications
FROM bank_loan_data
WHERE MONTH(issue_date) = 11;
```

## Total Funded Amount
```sql
SELECT SUM(loan_amount) AS Total_Funded_Amount
FROM bank_loan_data;
```

## MTD Total Funded Amount

```sql
SELECT SUM(loan_amount) AS Total_Funded_Amount
FROM bank_loan_data
WHERE MONTH(issue_date) = 12;
```

## PMTD Total Funded Amount

```sql
SELECT SUM(loan_amount) AS Total_Funded_Amount
FROM bank_loan_data
WHERE MONTH(issue_date) = 11;
```

## Average DTI

```sql
SELECT AVG(dti) * 100 AS Avg_DTI
FROM bank_loan_data;
```

## MTD Average DTI
```sql
SELECT AVG(dti) * 100 AS MTD_Avg_DTI
FROM bank_loan_data
WHERE MONTH(issue_date) = 12;
```

## PMTD Average DTI

```sql
SELECT AVG(dti) * 100 AS PMTD_Avg_DTI
FROM bank_loan_data
WHERE MONTH(issue_date) = 11;
```









