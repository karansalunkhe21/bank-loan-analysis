# 📊 Bank Loan Analysis – Power BI Dashboard

This Power BI project provides comprehensive analytics and insights into bank loan portfolios, enabling data-driven decision-making for loan approval, risk assessment, and portfolio management.

The dashboard analyzes loan applications across multiple dimensions including borrower demographics, creditworthiness, repayment behavior, and regional trends.

---

## 🚀 Project Overview

The objective of this project is to analyze bank loan data to:
- Monitor loan performance
- Identify high-risk borrowers
- Optimize loan portfolios
- Improve operational efficiency
- Support regulatory compliance

This project is built using **Power BI**, **Power Query**, and **DAX**, with structured documentation for scalability and maintainability.

---

## 🔑 Key Features

### 📌 Loan Performance Tracking
- Loan status analysis: **Fully Paid**, **Charged Off**, **Current**
- Portfolio-level performance monitoring

### ⚠️ Risk Assessment Dashboard
- Loan analysis by **grade** and **sub-grade**
- Credit risk indicators and borrower metrics

### 🌍 Geographic Analysis
- State-wise loan distribution
- Regional performance and default trends

### 👤 Borrower Profiling
- Employment length analysis
- Home ownership classification
- Income verification insights

### 💰 Financial Metrics
- Debt-to-Income (DTI) ratios
- Interest rate distribution
- Installment and total payment tracking

### 📈 Time-Series Analysis
- Loan issuance trends
- Payment behavior over time
- Loan aging analysis

---

## 🎯 Business Goals

- **Risk Management:** Identify high-risk loans and default patterns  
- **Portfolio Optimization:** Analyze performance across loan segments  
- **Customer Insights:** Understand borrower behavior and demographics  
- **Operational Efficiency:** Track loan processing and payment cycles  
- **Regulatory Compliance:** Monitor loan grading and verification status  

---

## 📁 Repository Structure

```text
bank-loan-analysis/
│
├── README.md                          # Project documentation
├── LICENSE                            # MIT License
├── .gitignore                         # Git ignore rules for Power BI
│
├── PowerBI/
│   ├── BANK_LOAN_ANALYSIS.pbix        # Power BI Desktop file
│ 
├── Data/
│   ├── financial_loan.csv             # Source dataset
│
├── Documentation/
│   ├── Domain_Knowledge.md            # Banking domain concepts
│   ├── Data_Terminologies.md          # Data field explanations
│   ├── data_dictionary.md             # Field definitions
│   ├── Data_Model.md                  # Data model design
│   └── User_Guide.md                  # Dashboard usage guide
│
├── Scripts/
│   ├── power_query_m/                 # Power Query (M) scripts
│   └── dax_measures/                  # DAX calculations
│
└── Images/
    ├── dashboard_preview.png          # Dashboard screenshot
    └── data_model_diagram.png         # Data model diagram
