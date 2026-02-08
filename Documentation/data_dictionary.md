
# 📖 Data Dictionary – Bank Loan Analysis

This data dictionary provides comprehensive documentation for all fields in the Bank Loan Analysis dataset. Each field includes its **definition, data type, business purpose, sample values, constraints, and analytics usage**.

---

## 🔑 Identifier Fields

| Field       | Data Type | Description                         | Constraints                       | Business Use | Sample Values | Analytics Use |
|------------|-----------|-------------------------------------|-----------------------------------|--------------|---------------|---------------|
| `id`       | Integer   | Unique identifier for each loan     | Primary Key, Non-null, Unique     | Track individual loans, reference for inquiries, join key | 1077430, 1072053 | Count distinct for total applications |
| `member_id`| Integer   | Unique identifier for borrower      | Non-null                          | Track borrower across loans, CRM, cross-sell opportunities | 1314167, 1288686 | Count distinct for unique customers |

---

## 📍 Geographic & Demographic Fields

| Field           | Data Type | Description                   | Constraints / Values                                           | Business Use | Sample Values | Analytics Use |
|----------------|-----------|-------------------------------|----------------------------------------------------------------|--------------|---------------|---------------|
| `address_state` | Text     | US state where borrower resides | 51 valid US states (AL…WY + DC)                               | Regional risk assessment, compliance, geographic portfolio | GA, CA, TX | Heat maps, state-wise performance, risk profiling |
| `application_type` | Text  | Type of loan application       | Only "Individual" or "Joint"                                    | Determine if loan is for single or joint applicant | Individual, Joint | Segmentation, performance comparison |

---

## 💼 Employment Information

| Field       | Data Type | Description                     | Constraints / Values           | Business Use | Sample Values | Analytics Use |
|------------|-----------|---------------------------------|-------------------------------|--------------|---------------|---------------|
| `emp_length` | Text     | Length of borrower's employment | 11 categories (<1 year, 1…10+ years) | Employment stability, risk factor | "< 1 year", "9 years" | Default rate by employment length |
| `emp_title`  | Text     | Borrower's job title            | Free text, high cardinality (~13,000 unique) | Income source verification, industry risk | "Ryder", "Tech Data Corp" | Industry segmentation, occupation-based risk |

---

## 🎯 Risk Classification Fields

| Field       | Data Type | Description                     | Constraints / Values | Business Use | Sample Values | Analytics Use |
|------------|-----------|---------------------------------|-------------------|--------------|---------------|---------------|
| `grade`     | Text      | Risk classification (loan)       | 7 values: A-G, Non-null | Pricing, risk management | A, B, C | Risk distribution, grade migration, ROI by tier |
| `sub_grade` | Text      | Refined risk within grade        | 35 values: A1-G5, Non-null | Granular risk & pricing | C4, E1 | Sub-grade performance, price sensitivity |

---

## 🏠 Home Ownership

| Field          | Data Type | Description                     | Values / Constraints | Business Use | Sample Values | Analytics Use |
|----------------|-----------|---------------------------------|-------------------|--------------|---------------|---------------|
| `home_ownership` | Text     | Borrower's residential status  | RENT, MORTGAGE, OWN, OTHER | Stability, default risk, collateral | RENT, MORTGAGE | Default rates, LTV analysis |

---

## 📅 Date Fields

| Field                  | Data Type | Description | Constraints / Rules | Business Use | Sample Values | Analytics Use |
|------------------------|-----------|------------|-------------------|--------------|---------------|---------------|
| `issue_date`           | Date      | Loan origination date | Non-null, year 2021 | Loan lifecycle, maturity, vintage analysis | 11-02-2021 | Cohort, monthly trends, aging |
| `last_credit_pull_date`| Date      | Last credit report check | ≥ issue_date | Credit monitoring, compliance | 13-09-2021 | Risk monitoring, credit review timing |
| `last_payment_date`    | Date      | Most recent payment | ≥ issue_date | Payment tracking, delinquency detection | 13-04-2021 | Days since last payment, cash flow |
| `next_payment_date`    | Date      | Expected next payment | > last_payment_date | Payment scheduling, cash flow | 13-05-2021 | Forecasting, adherence analysis |

---

## 💰 Financial Metrics

| Field           | Data Type | Description                     | Constraints / Notes | Business Use | Sample Values | Analytics Use |
|----------------|-----------|---------------------------------|-------------------|--------------|---------------|---------------|
| `loan_amount`   | Decimal   | Principal amount                | > 0, $1k-$40k     | Portfolio size, revenue, risk exposure | $2,500, $12,000 | Total portfolio, avg loan size |
| `int_rate`      | Decimal   | Interest rate charged           | 5%-35%            | Revenue, risk-based pricing | 0.1527 (15.27%) | Avg rate, yield by grade |
| `installment`   | Decimal   | Monthly payment (P+I)           | > 0                | Cash flow, affordability | $59.83, $421.65 | Avg payment, P/I ratio |
| `annual_income` | Decimal   | Borrower's gross income         | > 0, $15k-$300k+  | DTI, repayment capacity | $30,000, $83,000 | Income distribution, loan-to-income |
| `dti`           | Decimal   | Debt-to-Income ratio            | 0-0.50             | Risk metric, approval factor | 0.2088 | DTI vs default, risk segmentation |
| `total_payment` | Decimal   | Total paid to date              | ≤ loan + interest | Revenue recognition, recovery | $1,009, $3,939 | Payment progress, recovery rates |
| `total_acc`     | Integer   | Total credit accounts           | ≥ 0                | Credit experience, risk | 4, 28, 30 | Correlation w/ creditworthiness |

---

## 📊 Status & Classification Fields

| Field         | Data Type | Description                     | Values / Constraints | Business Use | Sample Values | Analytics Use |
|---------------|-----------|---------------------------------|-------------------|--------------|---------------|---------------|
| `loan_status` | Text      | Current loan status              | Fully Paid, Current, Charged Off | Portfolio assessment, risk | "Charged Off", "Fully Paid" | Good vs bad loans, provisioning |
| `purpose`     | Text      | Loan purpose                     | 14 categories | Risk & product analysis | "car", "debt_consolidation" | Purpose-based performance |
| `term`        | Text      | Loan repayment period            | 36 or 60 months | Cash flow, risk | "36 months", "60 months" | Term analysis, default risk |
| `verification_status` | Text | Income verification status      | Verified, Source Verified, Not Verified | Risk assessment, compliance | "Verified" | Approval & default rates |

---

## 🔗 Data Relationships

- **Loan-to-Member:** One member can have multiple loans (`member_id → id`)  
- **Grade-to-SubGrade:** Hierarchy (`A → A1-A5, B → B1-B5, …`)  
- **Date Hierarchy:** `issue_date → Year → Quarter → Month → Day`  
- **Geographic Hierarchy:** `Country (USA) → Region → State`

---

## 🧮 Calculated Fields (DAX Examples)

```DAX
// Loan Age (months)
Loan Age = DATEDIFF('financial_loan'[issue_date], TODAY(), MONTH)

// Income Segment
Income Segment = SWITCH(
    TRUE(),
    'financial_loan'[annual_income] < 35000, "Low",
    'financial_loan'[annual_income] < 75000, "Medium",
    'financial_loan'[annual_income] < 150000, "High",
    "Very High"
)

// DTI Risk Category
DTI Risk = SWITCH(
    TRUE(),
    'financial_loan'[dti] < 0.20, "Excellent",
    'financial_loan'[dti] < 0.30, "Good",
    'financial_loan'[dti] < 0.40, "Fair",
    "High Risk"
)

// Loan Type Flag
Loan Type = IF(
    'financial_loan'[loan_status] IN {"Fully Paid","Current"},
    "Good Loan",
    "Bad Loan"
)

// Payment Progress %
Payment Progress = DIVIDE(
    'financial_loan'[total_payment],
    'financial_loan'[loan_amount],
    0
)
