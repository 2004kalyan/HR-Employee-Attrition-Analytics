# HR Employee Attrition & Workforce Analytics

## Project Overview

**HR Employee Attrition & Workforce Analytics** is an end-to-end data analytics project built using the IBM HR Analytics Employee Attrition & Performance dataset.

The project analyzes employee attrition, workforce characteristics, overtime, compensation, tenure, job roles, departments, and other employee-related factors to identify patterns associated with employee turnover.

The project follows a practical analytics workflow:

**SQL Server → Python / Pandas → Exploratory Data Analysis → Business Analysis → Power BI / DAX → Dashboard → Business Insights**

---

## Business Problem

Employee turnover can increase recruitment costs, reduce workforce stability, and create operational challenges.

The objective of this project is to help HR and business stakeholders understand:

- How many employees left the organization?
- What is the overall attrition rate?
- Which departments and job roles contribute most to employee exits?
- How is attrition associated with overtime?
- How does tenure differ between employees who stayed and employees who left?
- How does compensation differ between employees who stayed and employees who left?
- Which workforce segments show higher observed attrition?
- Which patterns should HR investigate further?

The analysis focuses on identifying **associations and patterns**, not proving that any individual factor directly causes attrition.

---

## Project Objectives

1. Measure overall employee attrition.
2. Analyze attrition across departments and job roles.
3. Examine overtime and employee turnover patterns.
4. Analyze tenure and compensation differences.
5. Segment employees using business-relevant groups.
6. Perform deeper exploratory analysis using multiple variables.
7. Use SQL Server to answer business questions.
8. Build DAX measures for Power BI.
9. Develop an executive-style Power BI dashboard.
10. Translate analytical findings into actionable HR questions.

---

## Dataset

**Dataset:** IBM HR Analytics Employee Attrition & Performance

- Original records: **1,470 employees**
- Original columns: **35**
- Cleaned columns: **32**
- Grain: **1 row = 1 employee**
- Target/outcome: **Attrition**
- Database: `HRANALYTICS`
- SQL table: `dbo.HR_Employee_Attrition_Cleaned`

### Data Preparation

The following low-value or redundant columns were removed during preparation:

- `EmployeeCount`
- `EmployeeNumber`
- `Over18`
- `StandardHours`

The cleaned dataset was then used for Python analysis, SQL analysis, and Power BI.

---

## Analytical Workflow

### 1. Data Understanding

- Reviewed dataset structure and column meanings.
- Identified the employee-level grain.
- Classified variables by business meaning.
- Identified the attrition outcome.

### 2. Data Cleaning

- Removed redundant columns.
- Checked missing values.
- Checked data types.
- Created business-friendly analytical groups where required.

### 3. Exploratory Data Analysis

Python and Pandas were used to analyze:

- Attrition
- Overtime
- Job satisfaction
- Environment satisfaction
- Work-life balance
- Job involvement
- Business travel
- Distance from home
- Job level
- Monthly income
- Age
- Tenure
- Job role
- Department
- Marital status
- Total working years
- Years in current role
- Years with current manager

### 4. SQL Business Analysis

SQL Server was used to answer business questions that complement the Python analysis, including:

- Department contribution to total exits
- Job-role contribution to total exits
- Income comparison between employees who stayed and left
- Tenure comparison between employees who stayed and left
- Overtime patterns among employees who left
- High-exit workforce segments
- Role-level leaver characteristics
- Overtime proportion among leavers by job role

### 5. Power BI

Power BI was used for:

- KPI development
- DAX calculations
- Interactive filtering
- Workforce segmentation
- Executive dashboard development
- Business storytelling

---

## Exploratory Data Analysis — Python

### Overall Attrition

- Total employees: **1,470**
- Employees who left: **237**
- Employees who stayed: **1,233**
- Overall attrition rate: **16.12%**

### Selected Attrition Patterns

| Dimension | Observed Pattern |
|---|---|
| Overtime | Employees working overtime had substantially higher observed attrition than employees without overtime. |
| Job Satisfaction | Lower satisfaction levels showed higher observed attrition in several groups. |
| Environment Satisfaction | Lower satisfaction levels showed higher observed attrition. |
| Work-Life Balance | Employees in the lowest work-life balance category showed higher observed attrition. |
| Business Travel | Frequent travelers showed higher observed attrition than non-travel employees. |
| Distance From Home | Attrition increased across several distance groups, with the 20+ km group showing 22.06%. |
| Age | Employees aged 18–25 showed the highest observed attrition among the age groups analyzed. |
| Tenure | Employees with 0–2 years at the company showed the highest observed attrition among tenure groups. |
| Job Level | Job Level 1 showed relatively high observed attrition. |
| Job Role | Sales Representatives showed the highest observed attrition rate among the listed job roles. |
| Department | Sales showed 20.63% observed attrition, followed by HR at 19.05% and R&D at 13.84%. |
| Marital Status | Single employees showed higher observed attrition than married or divorced employees. |
| Total Working Years | Employees with 0–2 total working years showed the highest observed attrition. |

---

## Deeper EDA

### Overtime × Job Satisfaction

Overtime was associated with higher observed attrition across the job-satisfaction levels analyzed.

For example:

- Employees without overtime and Job Satisfaction 4: **6.94%**
- Employees with overtime and Job Satisfaction 4: **21.13%**
- Employees with overtime and Job Satisfaction 2: **37.68%**

This suggests that overtime deserves attention when evaluating employee turnover, including within satisfaction segments.

### Job Role × Overtime

Observed overtime exposure varied considerably by job role.

Examples:

- Sales Representative: **66.67%**
- Lab Technician: **50.00%**
- Research Scientist: **34.02%**
- Sales Executive: **32.98%**

### Tenure × Overtime

Employees in the early-tenure group had higher overtime exposure:

- 0–2 years: **50.96%**
- 3–5 years: **28.35%**
- 6–10 years: **25.83%**
- 11+ years: **10.77%**

This provides a useful segment for HR to investigate further.

### Job Level × Income

Job Level 1 contained a relatively large low-income segment. However, income did not show a simple monotonic relationship with attrition within every job level, so compensation should be interpreted together with role, tenure, overtime, and other workforce characteristics.

### Sales Representative × Job Level

Within Sales Representatives:

- Job Level 1: **42.11% observed attrition**
- Job Level 2: **14.29% observed attrition**

The smaller subgroup sizes should be considered when interpreting these percentages.

---

## SQL Server Business Analysis

The project used SQL Server to translate HR business questions into analytical queries.

### Key SQL Findings

#### Department Contribution to Exits

Among the 237 employees who left:

- R&D: **133 exits — 56.12%**
- Sales: **92 exits — 38.82%**
- HR: **12 exits — 5.06%**

These are **contributions to total exits**, not department-specific attrition rates.

#### Job-Role Contribution to Exits

The largest contributors to total exits were:

- Lab Technician: **62 — 26.16%**
- Sales Executive: **57 — 24.05%**
- Research Scientist: **47 — 19.83%**

Together, these three roles accounted for **70.04% of all recorded exits**.

#### Monthly Income Comparison

Average monthly income:

- Employees who stayed: **6,832**
- Employees who left: **4,787**
- Difference: approximately **2,045**

#### Tenure Comparison

Average years at company:

- Employees who stayed: **7 years**
- Employees who left: **5 years**
- Difference: approximately **2 years**

#### High-Exit Workforce Segments

One SQL analysis identified several high-exit combinations involving:

- Job Level
- Overtime
- Tenure

The largest segment was **Job Level 1 + Overtime + 0–2 years**, with **40 exits**.

However, another segment with **Job Level 1 + no Overtime + 0–2 years** also had **39 exits**. Therefore, the result should not be interpreted as overtime alone explaining the exits.

---

## Power BI & DAX

### Core Measures

```DAX
Total Employees =
COUNTROWS('HR_Employee_Attrition_Cleaned')
```

```DAX
Employees Left =
CALCULATE(
    [Total Employees],
    'HR_Employee_Attrition_Cleaned'[Attrition] = TRUE()
)
```

```DAX
Employees Stayed =
[Total Employees] - [Employees Left]
```

```DAX
Attrition Rate =
DIVIDE(
    [Employees Left],
    [Total Employees],
    0
)
```

```DAX
Average Monthly Income =
AVERAGE('HR_Employee_Attrition_Cleaned'[MonthlyIncome])
```

```DAX
Average Tenure =
AVERAGE('HR_Employee_Attrition_Cleaned'[YearsAtCompany])
```

```DAX
Overtime Employees =
CALCULATE(
    [Total Employees],
    'HR_Employee_Attrition_Cleaned'[OverTime] = TRUE()
)
```

```DAX
Overtime Rate =
DIVIDE(
    [Overtime Employees],
    [Total Employees],
    0
)
```

### Verified Power BI Metrics

- Total Employees: **1,470**
- Employees Left: **237**
- Employees Stayed: **1,233**
- Attrition Rate: **16.12%**
- Average Monthly Income: **6,502.93**
- Average Tenure: **7.01 years**
- Overtime Employees: **416**
- Overtime Rate: **28.30%**

---

## Final Power BI Dashboard

The final dashboard provides an executive view of:

- Workforce size
- Employee exits
- Overall attrition
- Workforce and compensation metrics
- Department patterns
- Job-role patterns
- Tenure patterns
- Overtime patterns
- Interactive filtering

The dashboard is designed to communicate the most important workforce patterns without presenting associations as proven causal drivers.

---

## Key Project Findings

1. **16.12% of employees in the dataset left the organization.**
2. **Overtime employees showed substantially higher observed attrition than non-overtime employees.**
3. **Employees with shorter tenure showed higher observed attrition.**
4. **Employees who left had lower average monthly income and shorter average tenure than employees who stayed.**
5. **Sales and HR had higher observed department-level attrition rates than R&D.**
6. **Sales Representatives showed the highest observed attrition rate among job roles.**
7. **The largest contributors to total exits were Lab Technicians, Sales Executives, and Research Scientists.**
8. **Early-tenure employees and Job Level 1 employees represent important workforce segments for further investigation.**
9. **Overtime exposure was considerably higher among employees with shorter tenure.**
10. **Some relationships were non-linear, so individual variables should not be interpreted in isolation.**

---

## Analytical Considerations

- The dataset is observational and does not establish causality.
- “Higher attrition” refers to an observed association within the dataset.
- Contribution to total exits and attrition rate are different metrics.
- Small job-role groups can produce unstable percentages.
- Segments should be interpreted using both percentages and employee counts.
- Multiple variables can overlap; therefore, a high-attrition segment should not automatically be treated as being caused by one factor.
- Findings describe this dataset and should not automatically be generalized to every organization.

---

## Tools & Technologies

- **Python**
- **Pandas**
- **NumPy**
- **Matplotlib**
- **SQL Server**
- **SSMS**
- **Power BI**
- **DAX**
- **Excel**
- **GitHub**

---

## Project Outcome

This project demonstrates an end-to-end approach to HR analytics:

**Business Problem → Data Preparation → Exploratory Analysis → Business Questions → SQL Analysis → DAX Measures → Power BI Dashboard → Business Insights**

The final outcome is an interactive HR analytics solution that converts employee-level data into structured workforce insights for HR and business stakeholders.
