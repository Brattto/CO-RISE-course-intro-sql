CO:RISE WEEK 4 PROJECT 

## Names of Students who have not registered for any course using outer joins
```sql
SELECT L.name, CRI.course_run_id
FROM learners AS L LEFT OUTER JOIN course_registration_info AS CRI ON (L.learner_id = CRI.learner_id)
WHERE CRI.course_run_id IS NULL;
```
## Names of Students who have not registered for any course using except Hint: Compare your output with the output of the previous query and register that Name is not a primary key of Student. 
```sql
SELECT learner_id 
FROM learners 
except
SELECT learner_id
FROM course_registration_info;
```
## Retrieve instructor_id and names of each instructor in corise and the  number of  courses they have taught
```sql
SELECT DISTINCT(I.name), I.instructor_id, COUNT(I.name)
FROM instructors AS I LEFT OUTER JOIN course_run as C ON(I.instructor_id = C.instructor_id)
GROUP BY I.name, I.instructor_id;
```
## Affiliations for which neither an instructor has taught a course in 2020 nor a student has registered for a course in 2020 (please do not use outer joins for this query)
```sql
SELECT I.affiliation
FROM instructors AS I JOIN course_run AS C ON (I.instructor_id = C.instructor_id)
WHERE NOT EXTRACT(YEAR FROM C.start_date) = 2020
INTERSECT
SELECT L.affiliation
FROM learners AS L JOIN course_registration_info AS CRI ON (L.learner_id = CRI.learner_id) JOIN course_run AS C ON (CRI.course_run_id = C.course_run_id)
WHERE NOT EXTRACT(YEAR FROM C.start_date) = 2020
ORDER BY affiliation;
```
## Average teaching experience of instructors who have never taught a class of more than 4 weeks
```sql
SELECT DISTINCT(I.name), AVG(I.teaching_experience) AS avg_teaching_experience_weeks
FROM instructors AS I FULL OUTER JOIN course_run AS C ON (I.instructor_id = C.instructor_id)
WHERE C.num_weeks <=4 AND I.name IS NOT NULL
GROUP BY I.name,C.num_weeks
ORDER BY I.name;
```
