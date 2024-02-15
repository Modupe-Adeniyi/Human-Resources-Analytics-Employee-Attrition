# Human Resources Analytics Employee Attrition

## Table of contents

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Preparation and Cleaning](#data-preparation-and-cleaning)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results](#results)
- [Recommendations](#recommendations)

## Project Overview

This project aims to conduct a comprehensive investigation into the dataset to reveal the factors that contribute to employee attrition. By analyzing various aspects of the available data, our objective is to assist the company in comprehending attrition within their organization. We aim to formulate data-driven recommendations focused on minimizing and preventing future employee attrition, thereby reducing turnover costs and enhancing employee retention over an extended period.



https://github.com/Modupe-Adeniyi/Human-Resources-Analytics-Employee-Attrition/assets/151841781/0d501d74-10f2-4527-9a9d-7e4097088464


## Data Sources

The primary dataset utilized for this analysis is the "HR_Employee_Attrition.csv" obtained from Kaggle. This dataset contains extensive information about employees, including age, gender, department, job role, educational field, salary, attrition, and various other details.

## Tools
PowerBI (Data cleaning and Creating reports)
SQL (Data analysis)

## Data Preparation and Cleaning

During the initial preparation phase, I executed the following activities:

-	Loading and inspecting the data
- Cleaning and formatting the data
- Modifying data types as needed.

 ## Exploratory Data Analysis

 Exploratory Data Analysis (EDA) involved examining the dataset to answer essential questions, including:
-	Identifying the gender with the highest attrition.
-	Determining the department with the highest attrition.
-	Analyzing the pattern in average salary for each department.
-	Pinpointing the age group at which attrition peaked.

## Data Analysis
Created MYSQL queries to analyze the dataset and extract insights. Some of the generated queries include:

1. Total employees
  ``` sql

SELECT
count(EmployeeNumber) as Total_Employees
FROM project.`employee attrition`;
```

2. Total Male employees
``` sql

SELECT
count(Gender) as Total_Male_Employees
FROM project.`employee attrition`
where Gender = "Male";
```

3. Percentage of male employees
``` sql

SELECT
round(
(SELECT
count(Gender) as Total_Male_Employees
FROM project.`employee attrition`
where Gender = "Male") / (SELECT
count(EmployeeNumber) as Total_Employees
FROM project.`employee attrition`) * 100, 0) as Male_Percentage;
```

4. Total Female employees
``` sql

SELECT
count(Gender) as Total_Female_Employees
FROM project.`employee attrition`
where Gender = "Female";
```

5. Percentage of female employees
``` sql

SELECT
round(
(SELECT
count(Gender) as Total_Female_Employees
FROM project.`employee attrition`
where Gender = "Female") / (SELECT
count(EmployeeNumber) as Total_Employees
FROM project.`employee attrition`) * 100, 0) as Female_Percentage;
```

6. Average Age
``` sql

SELECT 
round(avg(Age), 0) as Average_Age
FROM project.`employee attrition`;
```

7. Employee between ages 30 - 50
 ``` sql

SELECT 
Department,
JobRole,
EducationField,
MonthlyIncome,
age
FROM project.`employee attrition`
where age >= 30 and age <=50;
```

8. Average salary by department
``` sql

SELECT 
Department,
avg(MonthlyIncome) as Avg_monthly_income
FROM project.`employee attrition`
group by Department;
```

9. Number of employee by attrition
``` sql

SELECT
count(*) as Attrition_Count
FROM project.`employee attrition`
where Attrition = "yes";
```

10. Number of attrition by Gender
```sql

SELECT 
Gender,
count(Attrition) as Attrition_by_Gender
FROM project.`employee attrition`
where Attrition = "yes"
group by Gender;
```

11. Number of attrition by Department
``` sql

SELECT
Department,
count(*) as Attrition_Count
FROM project.`employee attrition`
where Attrition = "yes"
group by Department;
```

12. Unique job role
``` sql

SELECT
distinct JobRole
FROM project.`employee attrition`;
```

13. Total monthly income by gender
``` sql

SELECT
Gender,
sum(MonthlyIncome) as Gender_total_income
FROM project.`employee attrition`
group by Gender;
```

14. Highest monthly income
``` sql

SELECT
max(MonthlyIncome) as Highest_monthly_income
FROM project.`employee attrition`;
```

15. Highest monthly income by department
``` sql

SELECT
Department,
max(MonthlyIncome) as Highest_monthly_income
FROM project.`employee attrition`
group by Department;
```

16. Employee details with the highest monthly income
``` sql

SELECT *
FROM project.`employee attrition`
order by MonthlyIncome desc
limit 1;
```

17. Employees with the company for more than 10 years
``` sql
SELECT *
FROM project.`employee attrition`
where YearsAtCompany >= 10;
```

18. Average distance from home for employees in each department
``` sql

SELECT 
Department,
avg(DistanceFromHome) as Avg_distance_from_home
FROM project.`employee attrition`
group by Department;
```

19. Number of employee in each education field
``` sql

SELECT 
EducationField,
count(*) as Total
FROM project.`employee attrition`
group by EducationField;
```

20. Number of employee in each education field by attrition
``` sql

SELECT 
EducationField,
count(*) as Total
FROM project.`employee attrition`
where Attrition = "yes"
group by EducationField;
```

21. Average total working years for employee with high environmental satisfaction
``` sql

SELECT
avg(TotalWorkingYears) as Total_Avg
FROM project.`employee attrition`
where EnvironmentSatisfaction = (SELECT
max(EnvironmentSatisfaction)
FROM project.`employee attrition`);
```

22. Employees with the same educational field who has been in the company for more than 5 years
``` sql

SELECT e1.EmployeeNumber, 
e1.age,
e1.Department,
e1.YearsAtCompany,
e1.MonthlyIncome,
e1.EducationField
 from project.`employee attrition`e1
JOIN (
    SELECT 
	EducationField
    FROM project.`employee attrition`
    GROUP BY   EducationField
) e2 ON e1.EducationField = e2.EducationField
   where YearsAtCompany  > 5;
```
   
 23.   Departments where the average distance from  home is greater than the overall distance
``` sql

SELECT 
Department,
avg(DistanceFromHome) as Avg_distance_by_Dept,
(SELECT 
avg(DistanceFromHome) 
FROM project.`employee attrition`) as General_Avg_distance
  FROM project.`employee attrition`
group by Department
having  Avg_distance_by_Dept>
(SELECT 
avg(DistanceFromHome) 
  FROM project.`employee attrition`);
```

24. Employee who experienced atttrition and the  average total working years for this group
``` sql
SELECT 
EmployeeNumber,
Department,
Attrition,
TotalWorkingYears,
(SELECT
avg(TotalWorkingYears) as Avg_workng_years
FROM project.`employee attrition`
where Attrition = "yes") Attrition_Avg_workng_year
FROM project.`employee attrition`
where Attrition = "yes";
```

25. Employee with the lowest environmental satisfaction and their marital status.
``` sql

SELECT
EmployeeNumber,
EnvironmentSatisfaction,
MaritalStatus
FROM project.`employee attrition`
where EnvironmentSatisfaction = 
(SELECT 
min(EnvironmentSatisfaction)
FROM project.`employee attrition`);
```

26. Employees rank base on monthly income within each departmnt
``` sql

SELECT 
EmployeeNumber, department, MonthlyIncome, rank() over (partition by department order by MonthlyIncome desc) as rank_dept
FROM project.`employee attrition`;
```



https://github.com/Modupe-Adeniyi/Human-Resources-Analytics-Employee-Attrition/assets/151841781/6980188f-06b2-4d35-9484-2e7453310651

## Results

The analysis results are summarized as follows:
-	The total workforce comprised 1,470 employees.
-	The attrition rate for males (150) surpassed that of females (87), with males contributing to 63.29% of the total attrition.
-	Within departments, Research & Development experienced the highest total attrition (133), followed by Sales (92) and Human Resources (12). Research & Development accounted for 56.12% of the overall attrition.
-	The sum of monthly income for males was $5,627,608, exceeding that of females at $3,931,701. Males constituted 58.87% of the total monthly income.
-	Among age groups, Middle-Age Adults had the highest total attrition (103), followed by Adults (100) and Old Adults (34).
-	The Sales Department boasted the highest average monthly income at $6,959.17, trailed by Human Resources at $6,654.51 and Research & Development at $6,281.25. The lower average monthly income in the Research & Development Department likely contributed to its elevated attrition rate.
-	The peak attrition occurred among individuals aged 29 and 31, possibly due to their youth, familial responsibilities, and the pursuit of better opportunities and experiences. This age group exhibited a notably high attrition rate.

## Recommendations

1.	Gender-Related Attrition:
-	Investigate the reasons behind the higher attrition rate among males.
-	Conduct surveys or exit interviews to understand the specific challenges or concerns faced by male employees leading to attrition.
-	Implement targeted retention strategies or support programs to address the identified issues, fostering a more inclusive and supportive work environment for both genders.
2.	Departmental Attrition:
-	Analyze the factors contributing to the high attrition in the Research & Development department.
-	Consider a review of compensation structures and career development opportunities in Research & Development to align them with industry standards.
-	Implement initiatives to enhance employee engagement, recognition, and satisfaction within the Research & Development department to reduce attrition.
3.	Income Disparity:
-	Address the income disparity between male and female employees by reviewing and potentially adjusting salary structures.
-	Ensure transparent and fair practices in promotions and bonuses to bridge the income gap.
-	Promote gender equality in the workplace, emphasizing equal opportunities for career growth and advancement.
4.	Age-Related Attrition:
-	Focus on retention strategies for employees in the age group of 29 and 31.
-	Offer career development programs and advancement opportunities to retain talent in their prime working years.
-	Provide support for employees managing familial responsibilities, such as flexible work hours or family-friendly policies.
5.	Departmental Income Discrepancy:
-	Investigate and address the reasons behind the lower average monthly income in the Research & Development Department.
-	Assess and adjust salary structures in alignment with industry standards to make the department more competitive.
-	Implement measures to enhance the working conditions and overall job satisfaction within the Research & Development Department.
6.	Overall Employee Well-being:
-	Launch initiatives to support employee well-being, such as mental health programs, work-life balance policies, and stress management resources.
-	Foster a workplace culture that values and prioritizes the holistic well-being of employees, addressing factors beyond compensation that contribute to attrition.

By addressing these specific aspects, the organization can work towards creating a more inclusive, equitable, and supportive work environment, ultimately reducing attrition rates and enhancing overall employee satisfaction and retention.
