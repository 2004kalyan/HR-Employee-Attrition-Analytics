# HR Employee Attrition & Workforce Analytics — SQL Queries

Database: `HRANALYTICS`

Table: `dbo.HR_Employee_Attrition_Cleaned`

> `Attrition = 1` means the employee left.  
> `Attrition = 0` means the employee stayed.  
> `OverTime = 1` means the employee worked overtime.  
> `OverTime = 0` means the employee did not work overtime.

---

## 1. Total Employees

```sql
SELECT COUNT(*) AS TotalEmployees
FROM dbo.HR_Employee_Attrition_Cleaned;
```

---

## 2. Employees Who Left

```sql
SELECT COUNT(*) AS EmployeesLeft
FROM dbo.HR_Employee_Attrition_Cleaned
WHERE Attrition = 1;
```

---

## 3. Overall Attrition Rate

```sql
SELECT
    COUNT(CASE WHEN Attrition = 1 THEN 1 END) AS EmployeesLeft,
    COUNT(*) AS TotalEmployees,
    CAST(
        100.0 * COUNT(CASE WHEN Attrition = 1 THEN 1 END)
        / COUNT(*) AS DECIMAL(10,2)
    ) AS AttritionRate
FROM dbo.HR_Employee_Attrition_Cleaned;
```

---

## 4. Department Contribution to Total Exits

**Business question:** Which departments contribute the most to total employee exits?

```sql
WITH DepartmentExits AS
(
    SELECT
        Department,
        COUNT(*) AS EmployeesLeft
    FROM dbo.HR_Employee_Attrition_Cleaned
    WHERE Attrition = 1
    GROUP BY Department
)
SELECT
    Department,
    EmployeesLeft,
    CAST(
        100.0 * EmployeesLeft
        / SUM(EmployeesLeft) OVER()
        AS DECIMAL(10,2)
    ) AS ExitContributionPct
FROM DepartmentExits
ORDER BY EmployeesLeft DESC;
```

Expected result:

- R&D: 133 — 56.12%
- Sales: 92 — 38.82%
- HR: 12 — 5.06%

---

## 5. Job-Role Contribution to Total Exits

**Business question:** Which job roles contribute most to total exits?

```sql
WITH RoleExits AS
(
    SELECT
        JobRole,
        COUNT(*) AS EmployeesLeft
    FROM dbo.HR_Employee_Attrition_Cleaned
    WHERE Attrition = 1
    GROUP BY JobRole
)
SELECT
    JobRole,
    EmployeesLeft,
    CAST(
        100.0 * EmployeesLeft
        / SUM(EmployeesLeft) OVER()
        AS DECIMAL(10,2)
    ) AS ExitContributionPct
FROM RoleExits
ORDER BY EmployeesLeft DESC;
```

Expected top results:

- Lab Technician: 62 — 26.16%
- Sales Executive: 57 — 24.05%
- Research Scientist: 47 — 19.83%

---

## 6. Average Monthly Income — Stayed vs Left

**Business question:** How does average monthly income differ between employees who stayed and employees who left?

```sql
SELECT
    Attrition,
    COUNT(*) AS EmployeeCount,
    CAST(AVG(MonthlyIncome) AS DECIMAL(10,2)) AS AvgMonthlyIncome
FROM dbo.HR_Employee_Attrition_Cleaned
GROUP BY Attrition
ORDER BY Attrition;
```

Interpretation:

- `Attrition = 0`: employees who stayed
- `Attrition = 1`: employees who left

---

## 7. Average Tenure — Stayed vs Left

**Business question:** How does average company tenure differ between employees who stayed and employees who left?

```sql
SELECT
    Attrition,
    COUNT(*) AS EmployeeCount,
    CAST(AVG(CAST(YearsAtCompany AS DECIMAL(10,2))) AS DECIMAL(10,2)) AS AvgTenure
FROM dbo.HR_Employee_Attrition_Cleaned
GROUP BY Attrition
ORDER BY Attrition;
```

Expected result:

- Stayed: approximately 7 years
- Left: approximately 5 years

---

## 8. Overtime Pattern Among Employees Who Left

**Business question:** What percentage of employees who left had overtime?

```sql
SELECT
    OverTime,
    COUNT(*) AS EmployeesLeft,
    CAST(
        100.0 * COUNT(*)
        / SUM(COUNT(*)) OVER()
        AS DECIMAL(10,2)
    ) AS PercentageOfLeavers
FROM dbo.HR_Employee_Attrition_Cleaned
WHERE Attrition = 1
GROUP BY OverTime
ORDER BY OverTime;
```

---

## 9. Job Role Characteristics Among Leavers

**Business question:** What are the average income and tenure characteristics of employees who left, by job role?

```sql
SELECT
    JobRole,
    COUNT(*) AS EmployeesLeft,
    CAST(AVG(MonthlyIncome) AS DECIMAL(10,2)) AS AvgMonthlyIncome,
    CAST(AVG(CAST(YearsAtCompany AS DECIMAL(10,2))) AS DECIMAL(10,2)) AS AvgTenure
FROM dbo.HR_Employee_Attrition_Cleaned
WHERE Attrition = 1
GROUP BY JobRole
ORDER BY EmployeesLeft DESC;
```

Expected results include:

| Job Role | Exits | Avg Income | Avg Tenure |
|---|---:|---:|---:|
| Lab Technician | 62 | 2,919 | 3 |
| Sales Executive | 57 | 7,489 | 6 |
| Research Scientist | 47 | 2,780 | 4 |
| Sales Representative | 33 | 2,364 | 2 |
| HR | 12 | 3,715 | 4 |

Small job-role groups should be interpreted carefully.

---

## 10. Overtime Proportion Among Leavers by Job Role

**Business question:** Within each job role, what proportion of leavers had overtime?

```sql
SELECT
    JobRole,
    COUNT(*) AS EmployeesLeft,
    SUM(CASE WHEN OverTime = 1 THEN 1 ELSE 0 END) AS LeaversWithOvertime,
    CAST(
        100.0 * SUM(CASE WHEN OverTime = 1 THEN 1 ELSE 0 END)
        / COUNT(*)
        AS DECIMAL(10,2)
    ) AS OvertimePctAmongLeavers
FROM dbo.HR_Employee_Attrition_Cleaned
WHERE Attrition = 1
GROUP BY JobRole
ORDER BY OvertimePctAmongLeavers DESC;
```

When interpreting this output, always consider the number of leavers in each role.

---

## 11. Job Level × Overtime × Tenure Exit Segments

**Business question:** Which combinations of job level, overtime, and tenure contain the most exits?

```sql
SELECT
    JobLevel,
    OverTime,
    CASE
        WHEN YearsAtCompany BETWEEN 0 AND 2 THEN '0-2 years'
        WHEN YearsAtCompany BETWEEN 3 AND 5 THEN '3-5 years'
        WHEN YearsAtCompany BETWEEN 6 AND 10 THEN '6-10 years'
        ELSE '11+ years'
    END AS TenureGroup,
    COUNT(*) AS EmployeesLeft
FROM dbo.HR_Employee_Attrition_Cleaned
WHERE Attrition = 1
GROUP BY
    JobLevel,
    OverTime,
    CASE
        WHEN YearsAtCompany BETWEEN 0 AND 2 THEN '0-2 years'
        WHEN YearsAtCompany BETWEEN 3 AND 5 THEN '3-5 years'
        WHEN YearsAtCompany BETWEEN 6 AND 10 THEN '6-10 years'
        ELSE '11+ years'
    END
ORDER BY EmployeesLeft DESC;
```

Important interpretation:

The largest observed segment was:

**Job Level 1 + Overtime + 0–2 years = 40 exits**

However:

**Job Level 1 + No Overtime + 0–2 years = 39 exits**

Therefore, this result should not be interpreted as evidence that overtime alone explains attrition.

---

## 12. Department Attrition Rate

This is different from department contribution to total exits.

```sql
SELECT
    Department,
    COUNT(*) AS TotalEmployees,
    SUM(CASE WHEN Attrition = 1 THEN 1 ELSE 0 END) AS EmployeesLeft,
    CAST(
        100.0 * SUM(CASE WHEN Attrition = 1 THEN 1 ELSE 0 END)
        / COUNT(*)
        AS DECIMAL(10,2)
    ) AS AttritionRate
FROM dbo.HR_Employee_Attrition_Cleaned
GROUP BY Department
ORDER BY AttritionRate DESC;
```

Expected rates:

- Sales: 20.63%
- HR: 19.05%
- R&D: 13.84%

---

## 13. Job-Role Attrition Rate

```sql
SELECT
    JobRole,
    COUNT(*) AS TotalEmployees,
    SUM(CASE WHEN Attrition = 1 THEN 1 ELSE 0 END) AS EmployeesLeft,
    CAST(
        100.0 * SUM(CASE WHEN Attrition = 1 THEN 1 ELSE 0 END)
        / COUNT(*)
        AS DECIMAL(10,2)
    ) AS AttritionRate
FROM dbo.HR_Employee_Attrition_Cleaned
GROUP BY JobRole
ORDER BY AttritionRate DESC;
```

---

## 14. Overtime vs Attrition

```sql
SELECT
    OverTime,
    COUNT(*) AS TotalEmployees,
    SUM(CASE WHEN Attrition = 1 THEN 1 ELSE 0 END) AS EmployeesLeft,
    CAST(
        100.0 * SUM(CASE WHEN Attrition = 1 THEN 1 ELSE 0 END)
        / COUNT(*)
        AS DECIMAL(10,2)
    ) AS AttritionRate
FROM dbo.HR_Employee_Attrition_Cleaned
GROUP BY OverTime
ORDER BY AttritionRate DESC;
```

Expected pattern:

- No overtime: approximately 26.00% attrition
- Overtime: approximately 61.53% attrition

---

## 15. Tenure Group Attrition Rate

```sql
WITH TenureGroups AS
(
    SELECT
        CASE
            WHEN YearsAtCompany BETWEEN 0 AND 2 THEN '0-2 years'
            WHEN YearsAtCompany BETWEEN 3 AND 5 THEN '3-5 years'
            WHEN YearsAtCompany BETWEEN 6 AND 10 THEN '6-10 years'
            ELSE '11+ years'
        END AS TenureGroup,
        Attrition
    FROM dbo.HR_Employee_Attrition_Cleaned
)
SELECT
    TenureGroup,
    COUNT(*) AS TotalEmployees,
    SUM(CASE WHEN Attrition = 1 THEN 1 ELSE 0 END) AS EmployeesLeft,
    CAST(
        100.0 * SUM(CASE WHEN Attrition = 1 THEN 1 ELSE 0 END)
        / COUNT(*)
        AS DECIMAL(10,2)
    ) AS AttritionRate
FROM TenureGroups
GROUP BY TenureGroup
ORDER BY AttritionRate DESC;
```

---

## 16. Employees by Job Role and Overtime

```sql
SELECT
    JobRole,
    COUNT(*) AS TotalEmployees,
    SUM(CASE WHEN OverTime = 1 THEN 1 ELSE 0 END) AS OvertimeEmployees,
    CAST(
        100.0 * SUM(CASE WHEN OverTime = 1 THEN 1 ELSE 0 END)
        / COUNT(*)
        AS DECIMAL(10,2)
    ) AS OvertimeRate
FROM dbo.HR_Employee_Attrition_Cleaned
GROUP BY JobRole
ORDER BY OvertimeRate DESC;
```

---

## 17. SQL Concepts Demonstrated

This project demonstrates practical SQL concepts used in business analytics:

- `SELECT`
- `WHERE`
- `GROUP BY`
- `ORDER BY`
- `COUNT`
- `SUM`
- `AVG`
- `CASE`
- Conditional aggregation
- Common Table Expressions (`WITH`)
- Window functions
- `SUM() OVER()`
- Percentage calculations
- Business segmentation
- KPI calculation

---

## SQL Analysis Notes

### Contribution vs Rate

These two metrics answer different questions.

**Contribution to total exits:**

> Of all employees who left, what percentage came from this department or role?

**Attrition rate:**

> Of all employees in this department or role, what percentage left?

Both are useful, but they should never be presented as the same metric.

### Analytical Caution

SQL results describe patterns in this dataset. They do not establish that overtime, income, tenure, job role, or another variable independently caused employee attrition.

Always consider:

- Employee counts
- Segment size
- Overlapping variables
- Non-linear relationships
- Context from other analyses

