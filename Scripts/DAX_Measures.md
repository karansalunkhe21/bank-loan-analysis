# 📊 DAX Measures – Bank Loan Analysis

This document contains all **DAX measures** used in the Bank Loan Analysis Power BI model, organized by category.

---

## Base Measures

```dax
Total Loan Applications = COUNTROWS('financial_loan')

Total Funded Amount = SUM('financial_loan'[loan_amount])

Total Amount Received = SUM('financial_loan'[total_payment])

Average Interest Rate = AVERAGE('financial_loan'[int_rate])

Average DTI = AVERAGE('financial_loan'[dti])

