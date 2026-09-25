# HR Employee Attrition & Workforce Analytics — Project Report

## 1. Project Overview

**HR Employee Attrition & Workforce Analytics** is an end-to-end Data Analytics project focused on understanding employee attrition and identifying workforce segments associated with higher employee exits.

The project uses an employee-level HR dataset containing **1,470 employee records** and follows a complete analytics workflow:

**Python EDA → SQL Server Business Analysis → Power BI & DAX → Business Insights**

The objective was to convert employee data into business-focused insights that HR stakeholders could use to identify areas requiring further investigation and retention planning.

---

## 2. Business Problem

Employee attrition can increase recruitment costs, affect team stability, reduce productivity, and create workforce planning challenges.

The key business question addressed was:

> **Where is employee attrition concentrated, which workforce segments are associated with higher attrition, and which areas should HR investigate further to support employee retention?**

The project focuses on identifying associations and patterns rather than claiming that any single factor directly causes employees to leave.

---

## 3. Business Objectives

The analysis was designed to answer:

1. What is the overall employee attrition rate?
2. Which departments contribute the most employee exits?
3. Which job roles show higher observed attrition?
4. Is overtime associated with higher attrition?
5. Is attrition higher among newer employees?
6. How do income and tenure differ between employees who stayed and those who left?
7. Which combinations of job level, overtime, and tenure contain larger numbers of exits?
8. Which employee groups should HR investigate further?
9. How can the findings be communicated through an interactive dashboard?

---

## 4. Dataset

### Dataset

**IBM HR Analytics Employee Attrition & Performance dataset**

### Dataset Size

- Rows: **1,470 employees**
- Original columns: **35**
- Cleaned columns: **32**
- Grain: **1 row = 1 employee**
- Outcome variable: `Attrition`

### Important Variables

- Attrition
- Department
- JobRole
- JobLevel
- MonthlyIncome
- OverTime
- YearsAtCompany
- YearsInCurrentRole
- YearsWithCurrManager
- TotalWorkingYears
- JobSatisfaction
- EnvironmentSatisfaction
- WorkLifeBalance
- JobInvolvement
- BusinessTravel
- DistanceFromHome
- Age
- MaritalStatus
- StockOptionLevel
- NumCompaniesWorked
- TrainingTimesLastYear
- PerformanceRating
- EducationField

### Removed Low-Value / Redundant Columns

The following columns were removed during data preparation:

- EmployeeCount
- EmployeeNumber
- Over18
- StandardHours

---

## 5. Analytical Workflow

### Step 1 — Business Understanding

Defined the HR business problem, stakeholders, and questions the analysis needed to answer.

### Step 2 — Data Understanding

Inspected:

- Dataset dimensions
- Column names
- Data types
- Descriptive statistics
- Missing values
- Duplicate records
- Unique values and cardinality

### Step 3 — Data Cleaning

Performed data cleaning using Python and Pandas:

- Removed redundant columns
- Checked missing values
- Checked duplicates
- Validated data types
- Reviewed categorical values
- Created analytical groups for numerical variables

### Step 4 — Exploratory Data Analysis

Analyzed attrition across:

- Overtime
- Tenure
- Job role
- Department
- Job level
- Income
- Age
- Satisfaction
- Work-life balance
- Business travel
- Distance from home

### Step 5 — Deeper EDA

Performed cross-analysis including:

- Overtime × Job Satisfaction
- Job Role × Overtime
- Job Level × Income Group
- Tenure × Overtime
- Sales Representative × Job Level
- Job Level × Overtime × Tenure

### Step 6 — SQL Server Analysis

Loaded the cleaned dataset into SQL Server and answered business-focused questions using SQL.

### Step 7 — Power BI Dashboard

Built an interactive dashboard using Power BI and DAX to communicate workforce KPIs and attrition patterns.

---

# 6. Python Exploratory Data Analysis

Python was used for data cleaning, validation, exploratory analysis, grouping, and business insight generation.

### Libraries

- Pandas
- NumPy
- Matplotlib
- Seaborn

### Data Quality Checks

Performed:

- Dataset shape inspection
- Column inspection
- Data type validation
- Missing-value analysis
- Duplicate checks
- Unique-value analysis
- Descriptive statistics

The cleaned dataset contained **1,470 employee records** with no missing values after the data-preparation stage.

---

# 7. Overall Attrition

| Metric | Value |
|---|---:|
| Total Employees | 1,470 |
| Employees Who Left | 237 |
| Employees Who Stayed | 1,233 |
| Overall Attrition Rate | 16.12% |

The **16.12% attrition rate** provides the baseline against which employee segments were compared.

---

# 8. Key EDA Findings

## 8.1 Overtime

Observed attrition:

| Overtime | Attrition |
|---|---:|
| No | 26.00% |
| Yes | 61.53% |

Employees working overtime showed substantially higher observed attrition.

This identifies overtime as an important area for further HR investigation; it does not establish that overtime itself causes attrition.

---

## 8.2 Tenure

| Tenure | Attrition |
|---|---:|
| 0–2 years | 29.82% |
| 3–5 years | 13.82% |
| 6–10 years | 12.28% |
| 11+ years | 8.13% |

The **0–2 year group** showed substantially higher observed attrition than longer-tenure groups.

This makes early-tenure employee experience an important area for further investigation.

---

## 8.3 Job Role

Selected observed attrition rates:

| Job Role | Attrition |
|---|---:|
| Sales Representative | 39.76% |
| Lab Technician | 23.94% |
| Human Resources | 23.08% |
| Sales Executive | 17.48% |
| Research Scientist | 16.10% |

Sales Representatives showed the highest observed attrition rate among the major job roles analyzed.

---

## 8.4 Department

| Department | Attrition |
|---|---:|
| Sales | 20.63% |
| Human Resources | 19.05% |
| Research & Development | 13.84% |

It is important to distinguish **attrition rate** from **contribution to total exits**.

R&D contributed the largest number of exits because it has a large employee population, even though its attrition rate was lower than Sales.

---

## 8.5 Job Satisfaction

| Job Satisfaction | Attrition |
|---|---:|
| Level 1 | 22.84% |
| Level 4 | 11.33% |

Lower job-satisfaction levels showed higher observed attrition in the dataset.

---

## 8.6 Environment Satisfaction

| Environment Satisfaction | Attrition |
|---|---:|
| Level 1 | 25.35% |
| Level 4 | 13.45% |

Lower environment-satisfaction levels showed higher observed attrition.

---

## 8.7 Work-Life Balance

| Work-Life Balance | Attrition |
|---|---:|
| Level 1 | 31.25% |
| Level 2 | 16.86% |
| Level 3 | 14.22% |
| Level 4 | 17.65% |

The relationship is not perfectly monotonic, so the analysis does not claim that work-life balance alone determines attrition.

---

## 8.8 Business Travel

| Business Travel | Attrition |
|---|---:|
| Non-Travel | 8.00% |
| Travel_Rarely | 14.96% |
| Travel_Frequently | 24.91% |

Employees travelling frequently showed higher observed attrition.

---

## 8.9 Distance From Home

| Distance Group | Attrition |
|---|---:|
| 0–5 km | 13.77% |
| 6–10 km | 14.47% |
| 11–20 km | 20.00% |
| 20+ km | 22.06% |

Higher-distance groups showed higher observed attrition.

---

## 8.10 Job Level

| Job Level | Attrition |
|---|---:|
| Level 1 | 26.34% |
| Level 2 | 9.74% |
| Level 3 | 14.68% |
| Level 4 | 4.72% |
| Level 5 | 7.25% |

The relationship is not perfectly linear across job levels.

---

## 8.11 Monthly Income

A comparison of average monthly income showed:

| Employee Status | Avg. Monthly Income |
|---|---:|
| Stayed | ~6,833 |
| Left | ~4,787 |

Employees who left had approximately **2,045 lower average monthly income** than employees who stayed.

---

## 8.12 Age

| Age Group | Attrition |
|---|---:|
| 18–25 | 35.77% |
| 26–35 | 19.14% |
| 36–45 | 9.19% |
| 46–60 | 12.45% |

The 18–25 group showed substantially higher observed attrition.

---

# 9. Deeper EDA

## 9.1 Overtime × Job Satisfaction

Overtime employees showed higher observed attrition across multiple satisfaction levels.

Example:

- Satisfaction Level 4 + No Overtime: **6.94%**
- Satisfaction Level 4 + Overtime: **21.13%**

This provides additional context when investigating workload and employee experience.

---

## 9.2 Job Role × Overtime

Selected overtime rates:

| Job Role | Overtime |
|---|---:|
| Sales Representative | 66.67% |
| Lab Technician | 50.00% |
| Research Scientist | 34.02% |
| Sales Executive | 32.98% |

This highlights roles where workload patterns may deserve further investigation.

---

## 9.3 Tenure × Overtime

| Tenure | Overtime |
|---|---:|
| 0–2 years | 50.96% |
| 3–5 years | 28.35% |
| 6–10 years | 25.83% |
| 11+ years | 10.77% |

Overtime was particularly common among newer employees.

---

## 9.4 Sales Representative × Job Level

Among Sales Representatives:

- Job Level 1: **42.11% attrition**
- Job Level 2: **14.29% attrition**

Job level therefore provides additional context within a high-attrition role.

---

## 9.5 Job Level × Overtime × Tenure

One larger exit segment was:

**Job Level 1 + Overtime + 0–2 years = 40 exits**

Another similar segment was:

**Job Level 1 + No Overtime + 0–2 years = 39 exits**

This is an important analytical caution: the concentration of exits cannot be attributed to overtime alone because multiple workforce characteristics overlap.

---

# 10. SQL Server Business Analysis

The cleaned dataset was loaded into:

**Database:** `HRANALYTICS`

**Table:** `dbo.HR_Employee_Attrition_Cleaned`

SQL Server was used to answer business-focused questions and quantify important workforce patterns.

## SQL Questions Answered

1. Total employee count
2. Employees who left
3. Overall attrition rate
4. Department contribution to total exits
5. Job-role contribution to total exits
6. Average monthly income — stayed vs left
7. Average tenure — stayed vs left
8. Overtime patterns among employees who left
9. Characteristics of leavers by job role
10. Overtime proportion among leavers by job role
11. Job Level × Overtime × Tenure exit segments
12. Department attrition rate
13. Job-role attrition rate
14. Overtime vs attrition
15. Tenure-group attrition rate
16. Employees by job role and overtime

## SQL Concepts Demonstrated

- SELECT
- WHERE
- GROUP BY
- ORDER BY
- CASE
- Aggregate functions
- COUNT
- AVG
- SUM
- CTEs
- Window functions
- Conditional aggregation
- Percentage calculations

---

# 11. SQL Business Findings

## Department Contribution to Total Exits

Among the 237 employees who left:

| Department | Exits | Contribution |
|---|---:|---:|
| R&D | 133 | 56.12% |
| Sales | 92 | 38.82% |
| HR | 12 | 5.06% |

This represents **contribution to total exits**, not department-specific attrition rate.

---

## Job Role Contribution to Total Exits

Largest contributors:

| Job Role | Exits |
|---|---:|
| Lab Technician | 62 |
| Sales Executive | 57 |
| Research Scientist | 47 |
| Sales Representative | 33 |

The top three roles accounted for approximately **70.04% of all exits**.

---

## Average Income — Stayed vs Left

| Employee Status | Employees | Avg. Monthly Income |
|---|---:|---:|
| Stayed | 1,233 | ~6,833 |
| Left | 237 | ~4,787 |

Employees who left had lower average monthly income.

---

## Average Tenure — Stayed vs Left

| Employee Status | Employees | Avg. Tenure |
|---|---:|---:|
| Stayed | 1,233 | ~7 years |
| Left | 237 | ~5 years |

Employees who left had approximately two years lower average tenure.

---

# 12. Power BI Dashboard

Power BI was used to transform the analysis into a management-friendly dashboard.

## Key KPIs

The dashboard includes:

- Total Employees
- Employees Left
- Attrition Rate
- Average Monthly Income
- Average Tenure
- Overtime Rate

### Verified KPI Values

| KPI | Value |
|---|---:|
| Total Employees | 1,470 |
| Employees Left | 237 |
| Employees Stayed | 1,233 |
| Attrition Rate | 16.12% |
| Average Monthly Income | 6,502.93 |
| Average Tenure | 7.01 years |
| Overtime Employees | 416 |
| Overtime Rate | 28.30% |

---

# 13. DAX Measures

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

---

# 14. Dashboard Purpose

The final Power BI dashboard provides an executive-level view of employee attrition and workforce patterns.

It enables HR stakeholders to:

- Monitor overall attrition
- Understand workforce size and exits
- Examine attrition patterns across employee groups
- Identify departments and roles requiring further investigation
- Compare workforce characteristics
- Explore employee segments through dashboard filters

The dashboard is a **decision-support tool**, not a prediction or causal model.

---

# 15. Key Business Findings

### Finding 1 — Overall Attrition

Overall employee attrition was **16.12%**.

### Finding 2 — Overtime

Employees working overtime showed substantially higher observed attrition:

**61.53% vs 26.00%.**

### Finding 3 — Early Tenure

Employees with 0–2 years at the company showed **29.82% attrition**, compared with **8.13%** among employees with 11+ years.

### Finding 4 — High-Attrition Role

Sales Representatives showed **39.76% observed attrition**.

### Finding 5 — Exit Concentration

The top three job roles by number of exits accounted for approximately **70.04% of all employee exits**.

### Finding 6 — Income

Employees who left had approximately **2,045 lower average monthly income** than employees who stayed.

### Finding 7 — Tenure

Employees who left had approximately **2 years lower average tenure** than employees who stayed.

---

# 16. Business Recommendations / Areas for Further Investigation

## 1. Early-Tenure Retention

Investigate onboarding, manager support, career progression, workload, and employee experience during the first two years.

## 2. Overtime and Workload

Review overtime patterns by department, job role, tenure, and employee level to understand workload concentration.

## 3. High-Attrition Roles

Investigate why roles such as Sales Representative and Lab Technician show higher observed attrition.

## 4. Compensation

Review compensation patterns within comparable job roles and levels rather than treating income as an independent cause.

## 5. Employee Satisfaction

Investigate whether lower satisfaction levels are associated with specific teams, managers, roles, or workload conditions.

## 6. Workforce Segmentation

Use combinations of tenure, job level, overtime, role, and satisfaction to identify employee groups requiring closer investigation.

---

# 17. Analytical Considerations

This project is based on observational employee data.

Therefore:

- Association does not establish causation.
- High attrition in a group does not mean every employee in that group is at risk.
- Small groups should be interpreted cautiously.
- Attrition rate and contribution to total exits are different metrics.
- Counts and percentages answer different business questions.
- Multiple workforce characteristics can overlap.
- The analysis identifies patterns for investigation rather than proving why an employee leaves.

For example, overtime employees had higher observed attrition, but the analysis does not prove that overtime alone caused employees to leave.

---

# 18. Tools & Technologies

### Programming & Analysis
- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn

### Database & SQL
- Microsoft SQL Server
- SQL
- CTEs
- Window Functions
- Conditional Aggregation

### Business Intelligence
- Microsoft Power BI
- DAX
- Power Query

### Core Analytics Skills
- Data Cleaning
- Exploratory Data Analysis
- Business Question Formulation
- KPI Development
- Workforce Analytics
- Data Visualization
- Business Analysis
- Analytical Storytelling

---

# 19. Project Outcome

This project demonstrates an end-to-end approach to solving a business analytics problem:

**Raw HR Data**
→ **Data Cleaning**
→ **Exploratory Data Analysis**
→ **Business Questions**
→ **SQL Analysis**
→ **DAX & Power BI**
→ **Business Insights**
→ **Areas for Further Investigation**

The project converted employee-level data into a structured view of attrition patterns and workforce characteristics, providing HR stakeholders with a clearer basis for investigating retention opportunities.

---

# 20. Final Takeaway

> **Employee attrition was not evenly distributed across the workforce. Certain employee segments—particularly overtime employees, early-tenure employees, and selected job roles—showed higher observed attrition. By combining Python, SQL Server, and Power BI, the analysis transformed these workforce patterns into measurable business insights and an interactive decision-support dashboard.**

---

## Project Skills Demonstrated

**Python | Pandas | NumPy | EDA | SQL Server | Advanced SQL | Power BI | DAX | Data Cleaning | KPI Analysis | Workforce Analytics | Business Analysis | Data Visualization | Business Insights**
