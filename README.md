# Factors that Fuel Student Performance

### Project Overview

This project aims to explore the key factors that influence student success by analyzing a comprehensive dataset covering various aspects of student life, including study hours, sleep patterns, attendance, and more. Similar to how a city's transport system must adapt to meet residents' needs, this analysis helps educators and schools better understand how to support student achievement.

By uncovering the relationships between lifestyle habits and exam performance, the project provides actionable insights for students, teachers, and policymakers to make data-driven decisions that can improve academic outcomes and enhance educational strategies.

### Data Sources

The table we'll use for this project is called `student_performance` and includes the following data:

| Column                   | Definition                                                      | Data type             |
|--------------------------|-----------------------------------------------------------------|-----------------------|
| `attendance`              | Percentage of classes attended                                  |     `float`               |
| `extracurricular_activities` | Participation in extracurricular activities                   |     `varchar` (Yes, No)    |
| `sleep_hours`             | Average number of hours of sleep per night                      |     `float`               |
| `tutoring_sessions`       | Number of tutoring sessions attended per month                  |     `integer`             |
| `teacher_quality`         | Quality of the teachers                                         |     `varchar` (Low, Medium, High) |
| `exam_score`              | Final exam score                                                |     `float`               |

### Exploratory Data Analysis (EDA)

EDA involved exploring the manufacturing_parts table to answer key questions, such as:
- Do more study hours and extracurricular activities lead to better scores?
- Is there a sweet spot for study hours?
- How does sleep duration correlate with students' ranks — do top performers tend to get more, less, or balanced sleep?

### Data Analysis
Including some interesting code/features worked with

```sql
-- avg_exam_score_by_study_and_extracurricular
SELECT hours_studied,
       AVG(exam_score) AS avg_exam_score
FROM student_performance
WHERE hours_studied > 10 
  AND extracurricular_activities = 'Yes'
GROUP BY hours_studied
ORDER BY hours_studied DESC;
```

```sql
-- avg_exam_score_by_hours_studied_range
SELECT
  CASE
    WHEN hours_studied <= 5 THEN '1-5 hours'
    WHEN hours_studied <= 10 THEN '6-10 hours'
    WHEN hours_studied <= 15 THEN '11-15 hours'
    ELSE '16+ hours' END AS hours_studied_range,
  AVG(exam_score) AS avg_exam_score
FROM public.student_performance
GROUP BY hours_studied_range
ORDER BY avg_exam_score DESC;
```

```sql
-- student_exam_ranking
SELECT attendance,
       hours_studied,
       sleep_hours,
       tutoring_sessions,
       DENSE_RANK() OVER (ORDER BY exam_score DESC) AS exam_rank
FROM student_performance
ORDER BY exam_rank
LIMIT 30;
```

### Results/Findings

The result analysis are summarized as follows:
- The data indicates a positive correlation — as study hours increase beyond 10, combined with extracurricular involvement, the average exam score improves steadily.
- The 11–15 hours group achieves the highest average exam scores, outperforming both lower and higher study ranges.
- Students with balanced sleep (6–8 hours) dominate the top ranks, whereas those with very little (<5 hours) or excessive sleep (>9 hours) are underrepresented among top performers.

### Recommendations
Based on the analysis, we recommend the following actions:
- Encourage students to aim for 11–15 study hours per week, as this range consistently yields the highest exam performance.
- Promote participation in extracurricular activities alongside study, as combining both leads to improved academic outcomes.
- Educate students on the importance of balanced sleep, ideally between 6–8 hours per night, to support optimal learning and exam performance.
