# 📊 Data Model Documentation – Bank Loan Analysis

## Overview

The Bank Loan Analysis data model is designed as a **single-table model** with a star schema approach where all loan transaction data resides in one fact table. This simplified structure is suitable for this dataset size and provides excellent query performance.

---

## Data Model Architecture

### Model Type
- **Single Fact Table**: `financial_loan`
- **No Dimension Tables**: All attributes are denormalized in the fact table
- **Date Intelligence**: Built-in DAX date functions using `issue_date`

### Model Diagram

┌─────────────────────────────────────────┐
│ financial_loan (Fact) │
├─────────────────────────────────────────┤
│ Primary Key: │
│ • id (Integer) │
│ │
│ Dimensions (Attributes): │
│ • address_state │
│ • application_type │
│ • emp_length │
│ • emp_title │
│ • grade │
│ • sub_grade │
│ • home_ownership │
│ • loan_status │
│ • purpose │
│ • term │
│ • verification_status │
│ │
│ Date Fields: │
│ • issue_date (Date) │
│ • last_credit_pull_date (Date) │
│ • last_payment_date (Date) │
│ • next_payment_date (Date) │
│ │
│ Measures (Numeric): │
│ • loan_amount (Decimal) │
│ • funded_amount (Decimal) │
│ • int_rate (Decimal) │
│ • installment (Decimal) │
│ • annual_income (Decimal) │
│ • dti (Decimal) │
│ • total_payment (Decimal) │
│ • total_acc (Integer) │
└─────────────────────────────────────────┘

pgsql
Copy code

---

## Table Details

### `financial_loan` Table

**Type**: Fact Table  
**Granularity**: One row per loan application  
**Total Rows**: ~38,576 records  
**Total Columns**: 24  

#### Column Specifications

| Column Name          | Data Type | Description                        | Cardinality / Notes |
|----------------------|-----------|------------------------------------|-------------------|
| id                   | Integer   | Unique loan identifier (Primary Key) | 38,576 unique     |
| address_state        | Text      | US State abbreviation               | 51 values          |
| application_type     | Text      | Individual or Joint                 | 2 values           |
| emp_length           | Text      | Employment duration category        | 11 values          |
| emp_title            | Text      | Job title                           | ~13,000+ values    |
| grade                | Text      | Risk grade (A-G)                    | 7 values           |
| sub_grade            | Text      | Risk sub-grade (A1-G5)              | 35 values          |
| home_ownership       | Text      | Housing status                      | 4 values           |
| issue_date           | Date      | Loan origination date               | 365 dates          |
| last_credit_pull_date| Date      | Last credit check date              | 365 dates          |
| last_payment_date    | Date      | Most recent payment date            | 365 dates          |
| next_payment_date    | Date      | Expected next payment               | 365 dates          |
| loan_status          | Text      | Current status                      | 3 values           |
| member_id            | Integer   | Member identifier                   | 38,576 values      |
| purpose              | Text      | Loan purpose                        | 14 values          |
| term                 | Text      | Loan duration                       | 2 values           |
| verification_status  | Text      | Income verification                  | 3 values           |
| annual_income        | Decimal   | Borrower's yearly income             | Continuous         |
| dti                  | Decimal   | Debt-to-income ratio                 | Continuous         |
| installment          | Decimal   | Monthly payment                      | Continuous         |
| int_rate             | Decimal   | Interest rate                        | Continuous         |
| loan_amount          | Decimal   | Principal amount                     | Continuous         |
| total_acc            | Integer   | Total credit accounts                | Discrete           |
| total_payment        | Decimal   | Total amount paid                    | Continuous         |

---

## Relationships

**Note**: This model uses a single-table approach with **no explicit relationships**. All filtering and analysis is performed within the single fact table.

### Why No Dimension Tables?

1. **Data Size**: Dataset is manageable (~38K rows)  
2. **Simplicity**: Easier to maintain and understand  
3. **Performance**: No relationship overhead  
4. **Denormalization**: All attributes already present  

### Future Enhancements (Optional)

If the dataset grows significantly (>1M rows), consider creating dimension tables:

┌──────────────┐ ┌─────────────────┐ ┌──────────────┐
│ dim_Date │ │ financial_loan │ │ dim_State │
│──────────────│ │─────────────────│ │──────────────│
│ Date (PK) │◄──────│ issue_date (FK) │──────►│ StateCode(PK)│
│ Year │ │ loan_id (PK) │ │ StateName │
│ Quarter │ │ grade │ │ Region │
│ Month │ │ loan_amount │ └──────────────┘
│ Week │ │ ... │
└──────────────┘ └─────────────────┘
│
│
┌──────▼──────────┐
│ dim_LoanGrade │
│─────────────────│
│ Grade (PK) │
│ SubGrade │
│ Description │
│ RiskCategory │
└─────────────────┘

yaml
Copy code

---

## Data Transformations (Power Query)

### Applied Steps

1. **Source**: Load CSV file
```m
Source = Csv.Document(File.Contents([DataSourcePath] & "financial_loan.csv"))
Promoted Headers

m
Copy code
PromotedHeaders = Table.PromoteHeaders(Source)
Changed Types

m
Copy code
ChangedTypes = Table.TransformColumnTypes(PromotedHeaders, {
    {"id", Int64.Type},
    {"loan_amount", Currency.Type},
    {"int_rate", Percentage.Type},
    {"issue_date", type date},
    ...
})
Removed Duplicates

m
Copy code
RemovedDuplicates = Table.Distinct(ChangedTypes, {"id"})
Handled Nulls

m
Copy code
ReplacedNulls = Table.ReplaceValue(
    RemovedDuplicates,
    null,
    "Unknown",
    Replacer.ReplaceValue,
    {"emp_title"}
)
Calculated Columns

Month Name: Date.MonthName([issue_date])

Month Number: Date.Month([issue_date])

Year: Date.Year([issue_date])

DAX Measures
Base Measures
dax
Copy code
Total Loan Applications = COUNTROWS('financial_loan')
Total Funded Amount = SUM('financial_loan'[loan_amount])
Total Amount Received = SUM('financial_loan'[total_payment])
Average Interest Rate = AVERAGE('financial_loan'[int_rate])
Average DTI = AVERAGE('financial_loan'[dti])
Good vs Bad Loans
dax
Copy code
Good Loan Applications = 
    CALCULATE(COUNTROWS('financial_loan'),
    'financial_loan'[loan_status] IN {"Fully Paid","Current"})

Bad Loan Applications = 
    CALCULATE(COUNTROWS('financial_loan'),
    'financial_loan'[loan_status] = "Charged Off")

Good Loan % = DIVIDE([Good Loan Applications], [Total Loan Applications], 0)
Bad Loan % = DIVIDE([Bad Loan Applications], [Total Loan Applications], 0)
Month-over-Month Growth
dax
Copy code
MTD Total Funded Amount = CALCULATE([Total Funded Amount], DATESMTD('financial_loan'[issue_date]))
PMTD Total Funded Amount = CALCULATE([Total Funded Amount], DATESMTD(DATEADD('financial_loan'[issue_date], -1, MONTH)))
MoM Funded Amount = [MTD Total Funded Amount] - [PMTD Total Funded Amount]
MoM % = DIVIDE([MoM Funded Amount], [PMTD Total Funded Amount], 0)
Advanced Measures
dax
Copy code
Avg Loan by Grade = 
    AVERAGEX(VALUES('financial_loan'[grade]), CALCULATE([Total Funded Amount]))

Default Rate = 
    DIVIDE(
        CALCULATE(COUNTROWS('financial_loan'),'financial_loan'[loan_status]="Charged Off"),
        CALCULATE(COUNTROWS('financial_loan'),'financial_loan'[loan_status] IN {"Charged Off","Fully Paid"}),
        0
    )

Weighted Avg Interest = 
    DIVIDE(SUMX('financial_loan','financial_loan'[loan_amount]*'financial_loan'[int_rate]), [Total 

