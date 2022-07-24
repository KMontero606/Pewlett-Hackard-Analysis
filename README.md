# Pewlett-Hackard-Analysis
Deliverable 3: A written report on the employee database analysis

## Overview of the analysis:  
Two assignment reports were assigned to prepare for the upcoming "silver tsunami": 
* Determine the number of retiring employees per title, and 
* Identify employees who are eligible to participate in a mentorship program.

## Results: Provide a bulleted list with four major points from the two analysis deliverables. Use images as support where needed.
1. One major point of the analysis deliverable is creating a list that contains retiring employees. With the following following script:
```
SELECT e.emp_no,
e.first_name,
e.last_name,
te.title,
te.from_date,
te.to_date
INTO retirement_titles
FROM employees as e
INNER JOIN titles as te
ON (e.emp_no = te.emp_no)
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
ORDER BY te.emp_no ASC;

SELECT * FROM retirement_titles;
```
produces the following table:

<img width="497" alt="image" src="https://user-images.githubusercontent.com/106962921/180657121-453f9a32-ad00-4bc8-928b-585502e91518.png">

2. Second major point of the analysis is using the DISTINCT ON statement to remove duplicate entries. With the following script:
```
SELECT DISTINCT ON (emp_no) rt.emp_no,
rt.first_name,
rt.last_name,
rt.title

INTO unique_titles
FROM retirement_titles AS rt
WHERE (rt.to_date = '9999-01-01')
ORDER BY rt.emp_no, rt.to_date DESC;
```
produces the following table:

<img width="364" alt="image" src="https://user-images.githubusercontent.com/106962921/180657187-9a0b0bd7-d4ec-451c-bff1-2858ab1adc4a.png">

3. Third major point of the analysis is to count the number of retiring employees by their most recent job titles. With the following script:
```
SELECT COUNT(u.emp_no), u.title
INTO retiring_titles
FROM unique_titles AS u
GROUP BY u.title
ORDER BY u.count DESC;
```
produces the following table:

<img width="175" alt="image" src="https://user-images.githubusercontent.com/106962921/180657231-b9e7a865-5881-463e-98e4-e0252fc1327d.png">

4. Fourth major point of the analysis is to use DISTINCT ON statement to procure employees eligible for mentorship program. With the following script:
```
SELECT DISTINCT ON (emp_no) e.emp_no,
e.first_name,
e.last_name,
e.birth_date,
de.from_date,
de.to_date,
te.title 
INTO mentorship_eligibility
FROM employees AS e
INNER JOIN dept_emp as de
ON (e.emp_no = de.emp_no)	
INNER JOIN titles as te
ON (e.emp_no = te.emp_no)
WHERE (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31') 
AND (de.to_date = '9999-01-01')
ORDER BY e.emp_no, de.to_date DESC;
```
produces the following table:

<img width="559" alt="image" src="https://user-images.githubusercontent.com/106962921/180657259-fd8eead2-7a69-4c21-b176-2082fbfac0ce.png">

## Summary: Provide high-level responses to the following questions, then provide two additional queries or tables that may provide more insight into the upcoming "silver tsunami."

1. How many roles will need to be filled as the "silver tsunami" begins to make an impact?
   - There will be 72458 roles that will need to be filled as the "silver tsunami" begins to make an impact.
   ```
   SELECT COUNT(emp_no) FROM unique_titles;
   ```
   
   <img width="74" alt="image" src="https://user-images.githubusercontent.com/106962921/180658672-937277dd-7ac4-41ed-9f31-f93440657364.png">
   
2. Are there enough qualified, retirement-ready employees in the departments to mentor the next generation of Pewlett Hackard employees?
   - There are enough retirement-ready employees in the departments to mentor 1549 employees to start. Maintaining the mentorship program is crucial to ensure the retiring roles are filled so that more entry-level roles are created for incoming employees to fill. 
   ```
   SELECT COUNT(me.emp_no), me.title
   FROM mentorship_eligibility AS me
   GROUP BY me.title
   ORDER BY me.count DESC;
   ```

   <img width="175" alt="image" src="https://user-images.githubusercontent.com/106962921/180657765-b42dd3ee-a7ae-4aec-9012-680fc62500f6.png">
   <img width="72" alt="image" src="https://user-images.githubusercontent.com/106962921/180658740-a59ec3e9-cafc-463a-a267-7a6c1741798c.png">

