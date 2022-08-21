## - Co:RISE | Project Week 03

NOTE: The courses table has been replaced with the five tables discussed in the lessons. Please do not use courses table going forward in the projects.

## Find the maximum, average and standard deviation of the nps of a course run per each affiliation (of the instructor) provided there has been more than 11 (to make sure we are not susceptible to small number variations) course runs from instructors of that affiliation. 
```sql
SELECT max(course_run.nps) AS max_nps, avg(course_run.nps) AS avg_nps, stddev(course_run.nps) AS std_nps,instructors.affiliation
FROM instructors, course_run
WHERE instructors.instructor_id = course_run.instructor_id
GROUP BY instructors.affiliation
HAVING count(instructors.affiliation) > 11;
```

## Retrieve the maximum, average, and standard deviation of the nps of a course run for two levels of teaching experience – early, which is 5 or fewer years of teaching experience, and experienced, which is more than 5 years of experience. 

SELECT max(course_run.nps) AS max_nps, avg(course_run.nps) AS avg_nps, stddev(course_run.nps) AS std_nps,
case 
   when instructors.teaching_experience <= 5 then 'early'
   when instructors.teaching_experience > 5 then 'experienced'
   end AS experience
FROM instructors, course_run
WHERE instructors.instructor_id = course_run.instructor_id
GROUP BY experience;

## Find the maximum, average and standard deviation of the nps of a course run for early and experienced instructors combined with the level of the course.

SELECT max(C.nps) AS max_nps, avg(C.nps) AS avg_nps, stddev(C.nps) AS std_nps,CR.course_level,
case 
   when I.teaching_experience <= 5 then 'early'
   when I.teaching_experience > 5 then 'experienced'
   end AS experience
FROM instructors AS I JOIN course_run AS C on (I.instructor_id = C.instructor_id) JOIN course_info AS CR ON (C.course_id = CR.course_id)
WHERE I.instructor_id = C.instructor_id
GROUP BY CR.course_level, experience;

## Get the nps for each course run that has more than 2 learners from M & T Bank Corporation and is taught by an experienced instructor for a basic course (course level = 'B').

SELECT C.course_run_id,COUNT(C.course_run_id),C.nps
FROM instructors AS I JOIN course_run AS C on (I.instructor_id = C.instructor_id) JOIN course_info AS CR ON (C.course_id = CR.course_id) JOIN course_registration_info AS CRI on (CRI.course_run_id = C.course_run_id) JOIN learners AS L ON (CRI.learner_id = L.learner_id)
WHERE I.teaching_experience > 5 AND CR.course_level = 'B' AND L.affiliation = 'M & T Bank Corporation'
GROUP BY C.course_run_id, C.nps
HAVING COUNT(C.course_run_id) =2 ;

## Please give me the instructor with the highest variance in the nps scores of courses they have facilitated. This query should print only one row - output limited to one row

SELECT I.instructor_id, VARIANCE(C.nps)
FROM instructors AS I JOIN course_run AS C on (I.instructor_id = C.instructor_id)
GROUP BY I.instructor_id
HAVING VARIANCE(C.NPS) IS NOT NULL
ORDER BY variance DESC
LIMIT 1;
