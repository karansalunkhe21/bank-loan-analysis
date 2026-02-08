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


**Good vs Bad Loans**

```dax
Good Loan Applications = 
    CALCULATE(
        COUNTROWS('financial_loan'),
        'financial_loan'[loan_status] IN {"Fully Paid","Current"}
    )

Bad Loan Applications = 
    CALCULATE(
        COUNTROWS('financial_loan'),
        'financial_loan'[loan_status] = "Charged Off"
    )

Good Loan % = 
    DIVIDE([Good Loan Applications], [Total Loan Applications], 0)

Bad Loan % = 
    DIVIDE([Bad Loan Applications], [Total Loan Applications], 0)
