# HR_Analytics_Dashboard
Implemented a data-driven strategy for determining bonuses and incentives for healthy employees. Established a SQL database, utilized SQL queries for analysis, and constructed an interactive Power BI dashboard in alignment with wireframe specifications.
use hr_analytics.

## Create a join table
```sql
select * from Absenteeism_at_work as a
left join compensation as b
on a.ID = b.ID
left join Reasons as r
on a.Reason_for_absence = r.Number
```

## Find the healthiest employees for the bonus
```sql
select * from Absenteeism_at_work
where Social_drinker = 0 and Social_smoker = 0
and Body_mass_index < 25
and Absenteeism_time_in_hours < (select AVG(absenteeism_time_in_hours) from Absenteeism_at_work)
```
- Calculate a wage increase or annual compensation for non-smokers for insurance budget of $983,221 for all
- Total amount working hours = 2080 (5 days a week*8hrs*52weeks in a year) 
- 686 nonsmokers*2080hrs = 1,426,880
- Compensation rate increase for non-smokers/budget (983221/1426880)=.68 increase per hour.
- Compensation rate per each emp per year = 2080 (5 days a week*8hrs*52weeks in a year)*0.68 = $1414.4 per year.
```sql
Select count(*) as non_smokers from Absenteeism_at_work
where Social_smoker = 0
```
## Optimize query to avoid duplicate columns
```sql
select  
a.ID,
r.Reason,
Month_of_absence,
Body_mass_index,
Age,
CASE WHEN Body_mass_index < 18.5 THEN 'Underweight'
	 WHEN Body_mass_index BETWEEN 18.5 AND 25 THEN 'Healthy Weight'
	 WHEN Body_mass_index BETWEEN 25 AND 30 THEN 'Overweight'
	 WHEN Body_mass_index > 30 THEN 'Obese'
	 ELSE 'Unkown' end as BMI_Category,
CASE WHEN Month_of_absence in (12,1,2) then 'Winter'
	 WHEN Month_of_absence in (3,4,5) then 'Spring'
	 WHEN Month_of_absence in (6,7,8) then 'Summer'
	 WHEN Month_of_absence in (9,10,11) then 'Fall'
	 ELSE 'Unknown' END as Season_Name,
CASE WHEN age < 30 then '<30'
	 WHEN age BETWEEN 31 AND 39 then '31-39'
	 WHEN age BETWEEN 40 AND 49 then '40-49'
	 WHEN age BETWEEN 50 AND 59 then '50-59'
	 ELSE '60+' END as Age_Group,
Month_of_absence,
Day_of_the_week,
Transportation_expense,
Education,
Son,
Social_drinker,
Social_smoker,
Pet,
Disciplinary_failure,
Age,
Work_load_Average_day,
Absenteeism_time_in_hours
from Absenteeism_at_work as a
left join compensation as b
on a.ID = b.ID
left join Reasons as r
on a.Reason_for_absence = r.Number;
```
