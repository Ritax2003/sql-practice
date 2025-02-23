The website test contains 13 questions and consists of 3 category:

- Easy
- Medium
- Hard

You can use the following Entity Relationship Diagram (ERD) to figure out what data to store, the entities, their attributes and also how entities 
relate to other entities.

![image](https://github.com/gulbalasalamov/sql-practise.com/assets/19313466/c448fe34-44c4-4c12-a6aa-fcbce72aea0d)

## Answers

> Want to help this repository? Feel free to contribute by submitting a custom solution to be added to the questions.

---

### Section1: Easy

---

Questions 1- 9

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

8. Show the first_name, last_name. hire_date of the most recently hired employee.

```sql
SELECT first_name, last_name, hire_date
FROM employees
ORDER BY hire_date DESC
LIMIT 1;
```

9. Show the average unit price rounded to 2 decimal places, the total units in stock, total discontinued products from the products table.

```sql
SELECT ROUND(AVG(unit_price), 2)                     AS average_unit_price,
       SUM(units_in_stock)                           AS total_units_in_stock,
       SUM(CASE WHEN discontinued THEN 1 ELSE 0 END) AS total_discontinued_products
FROM products;
```

### Section2: Medium

---

Questions 1- 3

1. Show the ProductName, CompanyName, CategoryName from the products, suppliers, and categories table

```sql
SELECT p.product_name, s.company_name, c.category_name
FROM products p
JOIN categories c ON p.category_id = c.category_id
JOIN suppliers s ON p.supplier_id = s.supplier_id;
```

2. Show the category_name and the average product unit price for each category rounded to 2 decimal places.

```sql
SELECT c.category_name, ROUND(AVG(p.unit_price), 2) AS average_unit_price
FROM products p
JOIN categories c ON p.category_id = c.category_id
GROUP BY c.category_name;
```

3. Show the city, company_name, contact_name from the customers and suppliers table merged together.
   Create a column which contains 'customers' or 'suppliers' depending on the table it came from.

```sql
SELECT city, company_name, contact_name, 'customers' AS source
FROM customers
UNION ALL
SELECT city, company_name, contact_name, 'suppliers'
FROM suppliers;
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
