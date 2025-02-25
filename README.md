The website  contains  3 category:

- Easy
- Medium
- Hard

You can use the following Entity Relationship Diagram (ERD) to figure out what data to store, the entities, their attributes and also how entities 
relate to other entities.

![image](https://github.com/user-attachments/assets/7cbda51d-d631-4353-ab59-f8f4964a7d6f)


## Answers

> Want to help this repository? Feel free to contribute by submitting a custom solution to be added to the questions.

---

### Section1: Easy

---

1. Show first name, last name, and gender of patients whose gender is 'M'.

```sql
SELECT first_name,last_name,gender 
FROM patients 
where gender='M';
```

2. Show first name and last name of patients who does not have allergies. (null).

 ```sql
SELECT first_name,last_name
FROM patients 
where allergies is NULL;
```

3. Show first name of patients that start with the letter 'C' .

```sql
SELECT first_name
FROM patients 
where first_name like 'C%';
```

4. Show first name and last name of patients that weight within the range of 100 to 120 (inclusive).

```sql
SELECT first_name,last_name
FROM patients 
where weight between 100 and 120;
```

```sql
SELECT
  first_name,
  last_name
FROM patients
WHERE weight >= 100 AND weight <= 120;
```

5. Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'

```sql
update patients
set allergies='NKA'
where allergies is NULL;
```

6. Show first name and last name concatinated into one column to show their full name.

```sql
select
  concat(first_name, ' ', last_name) as full_name
from patients;
```
```sql
SELECT first_name || ' ' || last_name
FROM patients;
```
7. Show first name, last name, and the full province name of each patient.

Example: 'Ontario' instead of 'ON'

```sql
select p.first_name,p.last_name,pn.province_name
from patients p,province_names pn
where p.province_id=pn.province_id;
```

8. Show how many patients have a birth_date with 2010 as the birth year.

```sql
select count(*)
from patients 
where year(birth_date)=2010;
```

9. Show the first_name, last_name, and height of the patient with the greatest height.

```sql
select first_name,last_name,height
from patients 
where height = (select max(height) from patients);
```

10. Show all columns for patients who have one of the following patient_ids: 1,45,534,879,1000
```sql
select *
from patients 
where patient_id in (1,45,534,879,1000);
```

11. Show the total number of admissions. 

```sql
select count(*)
from admissions;
```
12. Show all the columns from admissions where the patient was admitted and discharged on the same day.

```sql
select *
from admissions 
where admission_date=discharge_date;
```
13. Show the patient id and the total number of admissions for patient_id 579..

```sql
select patient_id,count(*) as total_admissions
from admissions where patient_id=579;
```
14. Based on the cities that our patients live in, show unique cities that are in province_id 'NS'.

```sql
select distinct city 
from patients where province_id='NS';
```
```sql
SELECT city
FROM patients
GROUP BY city
HAVING province_id = 'NS';
```
15. Write a query to find the first_name, last name and birth date of patients who has height greater than 160 and weight greater than 70.

```sql
select first_name,last_name,birth_date 
from patients 
where height>160 and weight>70;
```
16. Write a query to find list of patients first_name, last_name, and allergies where allergies are not null and are from the city of 'Hamilton'.

```sql
select first_name,last_name,allergies
from patients 
where allergies is not NULL and city="Hamilton";
```

### Section2: Medium

---

1. Show unique birth years from patients and order them by ascending.

```sql
select distinct year(birth_date) as birth_years
from patients 
order by birth_years asc;
```

2. Show unique first names from the patients table which only occurs once in the list.
For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list.
If only 1 person is named 'Leo' then include them in the output.

```sql
SELECT first_name
FROM patients
GROUP BY first_name
HAVING COUNT(first_name) = 1
```
```sql
SELECT first_name
FROM (
    SELECT
      first_name,
      count(first_name) AS occurrencies
    FROM patients
    GROUP BY first_name
  )
WHERE occurrencies = 1
```

3. Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.

```sql
select patient_id,first_name
from patients 
where first_name like 's____%s' ;
```
```sql
SELECT
  patient_id,
  first_name
FROM patients
WHERE
  first_name LIKE 's%s'
  AND len(first_name) >= 6;
```
```sql
SELECT
  patient_id,
  first_name
FROM patients
where
  first_name like 's%'
  and first_name like '%s'
  and len(first_name) >= 6;
```
4. Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'. Primary diagnosis is stored in the admissions table.

```sql
select p.patient_id,p.first_name,p.last_name
from patients p,admissions a
where p.patient_id=a.patient_id and a.diagnosis='Dementia' ;

```
5. Display every patient's first_name. Order the list by the length of each name and then by alphabetically.

```sql
SELECT first_name
FROM patients
order by
  len(first_name),
  first_name;
```
6. Show the total amount of male patients and the total amount of female patients in the patients table.
Display the two results in the same row.

```sql
select(
    select count(*)
    from patients
    where gender = 'M'
  ) as male_count, (
    select count(*)
    from patients
    where gender = 'F'
  ) as female_count;

```
7. Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.

```sql
SELECT
  first_name,
  last_name,
  allergies
FROM patients
WHERE
  allergies IN ('Penicillin', 'Morphine')
ORDER BY
  allergies,
  first_name,
  last_name;
```
8. Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.

```sql
SELECT
  patient_id,
  diagnosis
FROM admissions
GROUP BY
  patient_id,
  diagnosis
HAVING COUNT(*) > 1;
```
9. Show the city and the total number of patients in the city.
Order from most to least patients and then by city name ascending..

```sql
select p.city, count(*)  as num_patients
from patients p
group by p.city
order by num_patients desc, p.city asc;
```
10. Show first name, last name and role of every person that is either patient or doctor.
The roles are either "Patient" or "Doctor"

```sql
select p.first_name,p.last_name,'Patient' as role
from patients p
union all
select d.first_name,d.last_name,'Doctor' as role
from doctors d;
```
8. Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.

```sql
SELECT
  patient_id,
  diagnosis
FROM admissions
GROUP BY
  patient_id,
  diagnosis
HAVING COUNT(*) > 1;
```

9. Show all allergies ordered by popularity. Remove NULL values from query.
```sql
SELECT allergies, count(*) as total_diagnosis 
FROM patients where allergies is not NULL
group by allergies order by total_diagnosis DESC ;

```
10. Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.

```sql
SELECT first_name,last_name,birth_date
FROM patients 
where year(birth_date)>=1970 and year(birth_date)<=1979
order by birth_date asc;

```
11. We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order
EX: SMITH,jane.

```sql
SELECT concat(upper(last_name),',',lower(first_name)) as new_name_format
FROM patients 
order by first_name desc;

```
12. Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.
    
```sql
select province_id,sum(height) 
from patients
group by province_id having sum(height)>=7000;
```
13. Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'
    
```sql
SELECT
  (MAX(weight) - MIN(weight)) AS weight_delta
FROM patients
WHERE last_name = 'Maroni';
```   

14. Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.
    
```sql
select day(admission_date) as day_number,
count(*) as number_of_admissions
from admissions
group by day_number 
order by number_of_admissions desc;
```

15. Show all columns for patient_id 542's most recent admission_date.
    
```sql
select *
from admissions
where patient_id is 542
and admission_date is (
  select max(admission_date) 
  from admissions
  where patient_id is 542);
```

16. Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:
1. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.
2. attending_doctor_id contains a 2 and the length of patient_id is 3 characters.
```sql
select
  patient_id,
  attending_doctor_id,
  diagnosis
from admissions
where
  (
    (patient_id % 2 is 1)
    and (attending_doctor_id in(1, 5, 19))
  )
  or (
    (attending_doctor_id like '%2%')
    and len(patient_id) is 3
  );
```
17. Show first_name, last_name, and the total number of admissions attended for each doctor. Every admission has been attended by a doctor.
    
```sql
select
  d.first_name,
  d.last_name,
  count(*) as admissions_total
from
  admissions a,
  doctors d
where a.attending_doctor_id=d.doctor_id
group by d.doctor_id;
```

18. For each doctor, display their id, full name, and the first and last admission date they attended.
    
```sql
select
  d.doctor_id,
  concat(d.first_name, " ", d.last_name) as full_name,
  min(a.admission_date) as first_admission_date,
  Max(a.admission_date) as last_admission_date
from doctors d, admissions a 
where d.doctor_id = a.attending_doctor_id
group by d.doctor_id;
```

19. Display the total amount of patients for each province. Order by descending.

```sql
SELECT
  province_name,
  COUNT(*) as patient_count
FROM patients pa
  join province_names pr on pr.province_id = pa.province_id
group by pr.province_id
order by patient_count desc;
```
20. For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.

```sql
select
  p.first_name || " " || p.last_name as patient_name,
  a.diagnosis,
  d.first_name || " " || d.last_name as doctor_name
from
  patients p,
  doctors d,
  admissions a
where
  p.patient_id = a.patient_id
  and a.attending_doctor_id = d.doctor_id;
```
21. display the first name, last name and number of duplicate patients based on their first name and last name.

Ex: A patient with an identical name can be considered a duplicate.

```sql
select
  first_name,
  last_name,
  count(*) as num_of_duplicates
from patients
group by first_name,last_name
having num_of_duplicates>1;
```
22. Display patient's full name,
height in the units feet rounded to 1 decimal,
weight in the unit pounds rounded to 0 decimals,
birth_date,
gender non abbreviated.

Convert CM to feet by dividing by 30.48.
Convert KG to pounds by multiplying by 2.205.

```sql
select
  first_name || ' ' || last_name as patient_name,
  round(height / 30.48, 1) as heightFeet,
  round(weight * 2.205, 0) as weightPound,
  birth_date,
  case
    when gender is 'M' then 'MALE'
    when gender is 'F' then 'FEMALE'
  end as gender_type
  from patients;
```

---

### Section3: Hard

---

Questions 1

1. Show the employee's first_name and last_name, a "num_orders" column with a count of the orders taken, and a column called "Shipped" that displays "On Time" if the order shipped_date is less or equal to the required_date, "Late" if the order shipped late. 
   Order by employee last_name, then by first_name, and then descending by number of orders.

```sql
SELECT e.first_name,
       e.last_name,
       COUNT(o.order_id)                                                          AS num_orders,
       CASE WHEN o.shipped_date <= o.required_date THEN 'On Time' ELSE 'Late' END AS shipped
FROM employees e
         JOIN orders o ON e.employee_id = o.employee_id
GROUP BY e.first_name, e.last_name, shipped
ORDER BY e.last_name, e.first_name, num_orders DESC;
```
