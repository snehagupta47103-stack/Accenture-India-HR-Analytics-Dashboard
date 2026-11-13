# Accenture India – HR Analytics Dashboard

## Page 1: Workforce Overview

A Power BI dashboard that gives a quick view of the overall workforce: headcount, department and gender split, attrition, tenure, hiring and a department-wise summary.

> **Data as on:** 04/11/2026
> **Tool:** Power BI Desktop

---

## Dashboard Preview 

<img width="1200" height="681" alt="Screenshot 2026-11-13 111653" src="https://github.com/user-attachments/assets/eb71c91c-054a-40aa-9b54-3bfdcda974df" />

--- 

## 1. Objective

To give HR leadership one page that answers:

- How many employees do we have and how many are active?
- How is the workforce spread across departments, locations and gender?
- What is the overall attrition rate and average tenure?
- How has hiring moved over time?

---

## 2. Dataset

11 CSV files (star schema): 6 dimension tables and 5 fact tables.

| Table | Type | Description |
|---|---|---|
| DimEmployee | Dimension | Employee ID, name, gender, department, location, job level (50,000 rows) |
| DimDepartment | Dimension | Department ID and name |
| DimLocation | Dimension | Location ID and city |
| DimJobLevel | Dimension | Job level ID and name |
| DimDate | Dimension | Date, year, quarter, month |
| FactWorkforce | Fact | Current salary, tenure, years of experience, active flag |
| FactAttrition | Fact | Employees who left and the reason |
| FactCompensation | Fact | Base salary, bonus, variable pay |
| FactPerformance | Fact | Performance rating, promoted and high performer flags |
| FactTraining | Fact | Training completion flag and course category |
| FactRecruitment | Fact | Candidate stage (Applied to Retained), date, department, location |

---

## 3. Data Model

Star schema with **DimEmployee** at the centre.

| From (One side) | To (Many side) |
|---|---|
| DimDepartment[DepartmentID] | DimEmployee[DepartmentID] |
| DimLocation[LocationID] | DimEmployee[LocationID] |
| DimJobLevel[JobLevelID] | DimEmployee[JobLevelID] |
| DimEmployee[EmployeeID] | FactWorkforce, FactCompensation, FactAttrition, FactPerformance, FactTraining |
| DimDate[DateKey] | FactRecruitment[DateKey] |
| DimDepartment[DepartmentID] | FactRecruitment[DepartmentID] |
| DimLocation[LocationID] | FactRecruitment[LocationID] |

Notes:

- FactAttrition has its own department, location and job level columns. They match DimEmployee 100%, so they are hidden and only `EmployeeID` is used for the relationship.
- FactRecruitment is **not** linked to DimEmployee because `EmployeeID` is blank for most candidates.
- DimDate is linked only to FactRecruitment, so Year and Quarter slicers affect only the Hiring Trend.

---

## 4. Measures used on this page

| Measure | Logic |
|---|---|
| Total Employees | `COUNTROWS(DimEmployee)` |
| Active Employees | Count of FactWorkforce rows where `IsActive = 1` |
| Total Attrition | Distinct count of `FactAttrition[EmployeeID]` |
| Attrition Rate % | Total Attrition / Total Employees |
| Retention Rate % | 1 - Attrition Rate % |
| Avg Tenure (Years) | Average of `FactWorkforce[TenureYears]` |
| Avg Experience (Years) | Average of `FactWorkforce[YearsExperience]` |
| Avg Salary (LPA) | Average of `CurrentSalary` / 100,000 |
| Male % / Female % | Gender-wise employees / Total Employees |
| New Hires | FactRecruitment rows where `Stage = "Joined"` |

---

## 5. Page 1 contents

**Filters (slicers):** Department, Quarter, Year, Business Unit, Location, Job Level, Gender

**KPI cards:** Total Employees, Active Employees, Attrition Rate %, Avg Tenure (Years)

**Visuals:**

| Visual | Type | What it shows |
|---|---|---|
| Employee Distribution by Department | Column chart | Employees per department |
| Workforce Composition | Donut chart | Male vs Female split |
| Hiring Trend | Line chart | New Hires by Year, Quarter and Month (drill-down), sorted by date |
| Workforce Summary | Matrix | By department: employees, attrition rate, avg experience, avg salary |
| Key Insights | Text box | Written takeaways (see below) |

**Navigation sidebar:** Workforce Overview (active), Attrition & Retention, Compensation Analytics, Performance & Promotion, Executive Summary.

---

## 6. Key Insights

1. Workforce stands at **50,000 employees**, of which **48,006 (96%)** are currently active.
2. Headcount is evenly spread across departments, with **SAP (3,477)** and **PMO (3,417)** being the largest.
3. Attrition rate is **16.0%** overall, ranging from 15.2% in Analytics to 16.8% in Finance.
4. Workforce is **60.8% male and 39.2% female**, with an average tenure of **10.3 years**.
5. Average experience is **12.8 years** and the average salary is about **₹31.45 LPA**.
6. Hiring has been steady at about **10,000 joinings each year** from 2022 to 2025.

---

## 7. Known limitations

- **Business Unit** is not in the dataset. It is created in DimDepartment by grouping departments (assumed mapping).
- **Age Group** and **Team** are not available in the data, so those visuals from the sample are skipped.
- **Attrition and Performance tables have no date**, so their trends cannot be built. Date-based trends use FactRecruitment only.
- **"vs PY" comparisons** are skipped because year-wise data is not available for most tables.
- **Active Employees and Total Attrition do not reconcile.** FactAttrition has 8,000 employees but only 1,994 are marked inactive in FactWorkforce. Attrition is taken from FactAttrition.
- Data is synthetic and departments and locations are almost evenly distributed, so charts look flatter than the sample dashboard.

---

## 8. How to open

1. Open the `.pbix` file in Power BI Desktop.
2. If data does not load, go to **Transform data > Data source settings** and point to the folder with the 11 CSV files.
3. Click **Refresh**.

---

## 9. Next pages

- Page 2: Attrition & Retention
- Page 3: Compensation Analytics
- Page 4: Performance & Promotion
- Page 5: Executive Summary

---

## 10. Acknowledgment

I would like to thank my mentors and trainers for their guidance and feedback throughout this project. The sample dashboard shared for reference helped me understand the structure and design of an HR analytics report. I also thank my friends and classmates for their support and suggestions.

The dataset used in this project is synthetic and has been used for learning purposes only. Accenture branding is used only as part of the reference design and no real company data is shown.

---

## 11. Author

| | |
|---|---|
| **Name** | Gupta Sneha |
| **Course** | Data Analysts |
| **Project** | Project 4: HR Analytics Dashboard |
| **Date** | 13/11/2026 |
