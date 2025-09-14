# Data-Science-Job-Analysis-SQL
Using PostgreSQL, I investigated data science job postings from 2024 to uncover insights for my job search.
# Data Analyst Job Market Exploration

## Introduction
Dive into the data job market!  
Focusing on data analyst roles, this project explores the **top-paying jobs**, **in-demand skills**, and where high demand meets high salary in data analytics.  

- SQL queries? Check them out here: `project sql folder` 💾

---

## Background
Driven by a quest to navigate the data analyst job market more effectively, this project was born from a desire to **pinpoint top-paid and in-demand skills**, streamlining work for others to find optimal jobs.  

Data hails from my SQL Course. It's packed with insights on job titles, salaries, locations, and essential skills.  

The questions I wanted to answer through my SQL queries were:  
1. What are the top-paying data analyst jobs?  
2. What skills are required for these top-paying jobs?  
3. What skills are most in demand for data analysts?  
4. Which skills are associated with higher salaries?  
5. What are the most optimal skills to learn?  

---

## Tools I Used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:  

- **SQL**: The backbone of my analysis, allowing me to query the database and unearth critical insights.  
- **PostgreSQL**: The chosen database management system, ideal for handling the job posting data.  
- **Visual Studio Code**: My go-to for database management and executing SQL queries.  
- **Git & GitHub**: Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.  

---

## The Analysis
Each query for this project aimed at investigating specific aspects of the data analyst job market. Here's how I approached each question:  

---

### 1. Top-Paying Data Analyst Jobs
To identify the highest-paying roles, I filtered data analyst positions by average yearly salary and location, focusing on remote jobs. This query highlights the high-paying opportunities in the field.  

```sql
SELECT
    job_id,
    job_title,
    salary_year_avg,
    name AS company_name
FROM job_postings_fact
LEFT JOIN company_dim 
    ON job_postings_fact.company_id = company_dim.company_id
WHERE job_title_short = 'Data Analyst'
  AND job_location = 'Anywhere'
  AND salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10;
```
## Breakdown of Results

- **Wide Salary Range**: Top 10 paying data analyst roles span from **$184,000 to $650,000**.  
- **Diverse Employers**: Companies like **SmartAsset, Meta, and AT&T** are among those offering high salaries.  
- **Job Title Variety**: Roles range from *Data Analyst* to *Director of Analytics*.  


  
