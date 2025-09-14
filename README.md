# Data-Science-Job-Analysis-SQL
Using PostgreSQL, I investigated data science job postings from 2024 to uncover insights for my job search.
# Data Analyst Job Market Exploration

## Introduction
Dive into the data job market!  
Focusing on data analyst roles, this project explores the **top-paying jobs**, **in-demand skills**, and where high demand meets high salary in data analytics.  

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


Breakdown of the Most Demanded Skills for the Top 10 Highest Paying Data Analyst Jobs in 2023
SQL is leading with a count of 8.
Python follows closely with a count of 7.
Tableau is also highly sought after, with a count of 6.
Other skills like R, Snowflake, Pandas, and Excel show varying degrees of demand
  

## 2. Skills for Top Paying Jobs  

To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.  

```sql
WITH top_paying_jobs AS (
    SELECT
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE job_title_short = 'Data Analyst'
      AND job_location = 'Anywhere'
      AND salary_year_avg IS NOT NULL
    ORDER BY salary_year_avg DESC
    LIMIT 10
)
SELECT top_paying_jobs.*,
       skills_dim.skill_name
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY salary_year_avg DESC;
```
## Breakdown of the Most Demanded Skills for the Top 10 Highest Paying Data Analyst Jobs in 2023  

- **SQL** is leading with a count of **8**.  
- **Python** follows closely with a count of **7**.  
- **Tableau** is also highly sought after, with a count of **6**.  
- Other skills like **R**, **Snowflake**, **Pandas**, and **Excel** show varying degrees of demand.  

### Skills Frequency Table  

| Skill       | Count |
|-------------|-------|
| SQL         | 8     |
| Python      | 7     |
| Tableau     | 6     |
| R           | Varies|
| Snowflake   | Varies|
| Pandas      | Varies|
| Excel       | Varies|

## 3. In-Demand Skills for Data Analysts  

This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.  

### SQL Query  
```sql
SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND job_work_from_home = True
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```
Breakdown of the Most Demanded Skills for Data Analysts in 2023
SQL – most frequently requested.
Excel – consistently valued in job postings.
Python – crucial for data manipulation and analysis.
Tableau – highly requested for visualization.
R – important for statistical analysis.

Bar Graph (Text Visualization)
SQL       ██████████████████████  (High demand)
Excel     ████████████████████    (Strong demand)
Python    ██████████████████      (Strong demand)
Tableau   ████████████████        (Moderate demand)
R         ████████████            (Moderate demand)

## 4. Skills Based on Salary  

Exploring the average salaries associated with different skills revealed which skills are the highest paying.  

### Table of the Demand for the Top 5 Skills in Data Analyst Job Postings  

| **Skill**   | **Demand Count** |
|-------------|------------------|
| SQL         | 7,291            |
| Excel       | 4,611            |
| Python      | 4,330            |
| Tableau     | 3,745            |
| Power BI    | 2,609            |

###  Notes  

- **SQL and Excel** remain fundamental, emphasizing the need for strong foundational skills in data processing and spreadsheet manipulation.  
- **Programming and Visualization Tools** like Python, Tableau, and Power BI are essential, pointing towards the increasing importance of technical skills in data storytelling and decision support.  

### SQL Query  

```sql
SELECT
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM
    job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True
GROUP BY
    skills
ORDER BY
    avg_salary DESC;
```
## Breakdown of Results: Top Paying Skills for Data Analysts  

- **High Demand for Big Data & ML Skills**: Top salaries are commanded by analysts skilled in big data technologies (PySpark, Couchbase), machine learning tools (DataRobot, Jupyter), and Python libraries (Pandas, NumPy), reflecting the industry's high valuation of data processing and predictive modeling capabilities.  
- **Software Development & Deployment Proficiency**: Knowledge in development and deployment tools (GitLab, Kubernetes, Airflow) indicates a lucrative crossover between data analysis and engineering, with a premium on skills that facilitate automation and efficient data pipeline management.  
- **Cloud Computing Expertise**: Familiarity with cloud and data engineering tools (Elasticsearch, Databricks, GCP) underscores the growing importance of cloud-based analytics environments, suggesting that cloud proficiency significantly boosts earning potential in data analytics.  

### Table: Average Salaries for Top Paying Skills  

| **Skill**     | **Average Salary ($)** |
|---------------|-------------------------|
| PySpark       | 208,172                 |
| Bitbucket     | 189,155                 |
| Couchbase     | 160,515                 |
| Watson        | 160,515                 |
| DataRobot     | 155,486                 |
| GitLab        | 154,500                 |
| Swift         | 153,750                 |
| Jupyter       | 152,777                 |
| Pandas        | 151,821                 |

## 5. Most Optimal Skills to Learn  

Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries, offering a strategic focus for skill development.  

### SQL Query  
```sql
SELECT
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst'
  AND salary_year_avg IS NOT NULL
  AND job_work_from_home = True
GROUP BY skills_dim.skill_id
HAVING COUNT(skills_job_dim.job_id) > 10
ORDER BY avg_salary DESC, demand_count DESC
LIMIT 25;
```
 Most Optimal Skills for Data Analysts Sorted by Salary
| Skill ID | Skill       | Demand Count | Average Salary ($) |
|----------|------------|--------------|------------------|
| 8        | Go         | 27           | 115,320          |
| 234      | Confluence | 11           | 114,210          |
| 97       | Hadoop     | 22           | 113,103          |
| 80       | Snowflake  | 37           | 112,948          |
| 74       | Azure      | 34           | 111,225          |
| 77       | BigQuery   | 13           | 109,654          |
| 76       | AWS        | 32           | 108,317          |
| 4        | Java       | 17           | 106,906          |
| 194      | SSIS       | 12           | 106,683          |
| 233      | Jira       | 20           | 104,918          |

## Table of the Most Optimal Skills for Data Analysts Sorted by Salary  

### Breakdown of the Most Optimal Skills for Data Analysts in 2023  

- **High-Demand Programming Languages**: Python and R stand out for their high demand, with demand counts of 236 and 148 respectively. Despite their high demand, their average salaries are around **$101,397** for Python and **$100,499** for R, indicating that proficiency in these languages is highly valued but also widely available.  
- **Cloud Tools and Technologies**: Skills in specialized technologies such as Snowflake, Azure, AWS, and BigQuery show significant demand with relatively high average salaries, pointing towards the growing importance of cloud platforms and big data technologies in data analysis.  
- **Business Intelligence and Visualization Tools**: Tableau and Looker, with demand counts of 230 and 49 respectively, and average salaries around **$99,288** and **$103,795**, highlight the critical role of data visualization and business intelligence in deriving actionable insights from data.  
- **Database Technologies**: The demand for skills in traditional and NoSQL databases (Oracle, SQL Server, NoSQL) with average salaries ranging from **$97,786** to **$104,534**, reflects the enduring need for data storage, retrieval, and management expertise.  

### What I Learned  

Throughout this adventure, I’ve turbocharged my SQL toolkit with some serious firepower:  
- **Complex Query Crafting**: Mastered the art of advanced SQL, merging tables like a pro and wielding `WITH` clauses for ninja-level temp table maneuvers.  
- **Data Aggregation**: Got cozy with `GROUP BY` and turned aggregate functions like `COUNT()` and `AVG()` into my data-summarizing sidekicks.  
- **Analytical Wizardry**: Leveled up my real-world puzzle-solving skills, turning questions into actionable, insightful SQL queries.  

### Conclusions & Insights  

From the analysis, several general insights emerged:  
 ** Top-Paying Data Analyst Jobs**: The highest-paying jobs for data analysts that allow remote work offer a wide range of salaries, the highest at **$650,000**.  
 ** Skills for Top-Paying Jobs**: High-paying data analyst jobs require advanced proficiency in **SQL**, suggesting it's a critical skill for earning a top salary.  
 ** Most In-Demand Skills**: SQL is also the most demanded skill in the data analyst job market, thus making it essential for job seekers.  
 ** Skills with Higher Salaries**: Specialized skills, such as **SV** and **Solidity**, are associated with the highest average salaries, indicating a premium on niche expertise.  

## Closing Thoughts  

This project enhanced my **SQL skills** and provided valuable insights into the **data analyst job market**. The findings from the analysis serve as a guide to **prioritizing skill development and job search efforts**.  

Aspiring data analysts can better position themselves in a competitive job market by focusing on **high-demand, high-salary skills**.  

This exploration highlights the importance of **continuous learning** and **adaptation to emerging trends** in the field of data analytics.


