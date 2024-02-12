# Analyzing Job Market Data in Power BI

### Project Overview
This project aims to analyze job market data collected from jobinja.ir using Python web scraping techniques and visualize insights using Power BI. The analysis includes salary trends, skill requirements, and job distribution across different categories and provinces.

### Web Scraping

The project utilized Python along with SQLAlchemy and SQLite3 to scrape job market data from jobinja.ir, resulting in the collection of information on approximately 18000 job listings across the country.

Link of the code(i did some small changes to the code for this project): https://github.com/ehsanSh21/jobinja_web_scraping

#### Steps Taken
1. **Web Scraping Process**: Employed Python scripts to scrape job data from jobinja.ir.
2. **Data Collection**: Gathered job information, including title, company, skills, category, and salary (where available).
3. **Data Storage**: Utilized SQLite3 database with SQLAlchemy ORM for efficient data storage.

### Data Preprocessing

To handle missing salary values, I estimated salaries for approximately 60% of job listings based on experience years, category, and province using SQL queries. The estimated salary was added as a new column named estimated_salary in the SQLite3 database.
##### SQL query: 
```sql
WITH numbered_jobs AS (
    SELECT
        id,
        category,
        experience_years_min,
        experience_years_max,
        salary_min,
        province,
        CASE
            WHEN experience_years_min = 0 AND experience_years_max = 0 THEN 1
            WHEN experience_years_min = 0 AND experience_years_max = 3 THEN 2
            WHEN experience_years_min = 3 AND experience_years_max = 6 THEN 3
            WHEN experience_years_min = 6 AND experience_years_max = 0 THEN 4
        END AS row_number
    FROM
        jobs
),
calc_new_salary AS (
    SELECT
        nj.*,
        CASE
            WHEN nj.salary_min = "توافقی" THEN
                (SELECT SUM(salary_min) / COUNT(CASE WHEN t2.salary_min != "توافقی" THEN 1 END)
                 FROM numbered_jobs t2
                 WHERE nj.category = t2.category
                 AND nj.experience_years_min = t2.experience_years_min
                 AND nj.experience_years_max = t2.experience_years_max
                 and nj.province = t2.province
                 )
            ELSE nj.salary_min
        END AS new_salary
    FROM
        numbered_jobs nj
    ORDER BY
        nj.experience_years_min, nj.experience_years_max
)
SELECT
    *,
    (CASE when t3.new_salary is NULL then
        (SELECT AVG(t4.new_salary)
     FROM calc_new_salary t4
     WHERE t3.category = t4.category
     and t3.province=t4.province
     )
        when t3.new_salary is not null then t3.new_salary
        end ) as estimated_salary
FROM
    calc_new_salary t3
;
```

### Data Model
The data model represents the schema used for storing job market data in the SQLite3 database. It consists of four main entities: Jobs, Skills, Companies, and Job_Skills, with defined relationships between them.
#### Diagram

<img src="https://github.com/ehsanSh21/jobinja-data-modeling/blob/main/dataModel.png" alt="Database Diagram" width="900" height="300">



### Data Visualization with Power BI
#### Overview
Power BI was employed to visualize and analyze the collected job market data, providing interactive and insightful visualizations.

#### Visualizations
- **Average Salary by Top 10 Categories**: Dynamic visualization showcasing the average salary for the top 10 categories, with a slicer for selecting specific provinces( determined salary).
<p float="left">
  <img src="https://github.com/ehsanSh21/jobinja-data-modeling/blob/main/avg_salary.png" alt="Database Diagram" width="360" height="350">
  <img src="https://github.com/ehsanSh21/jobinja-data-modeling/blob/main/avg_province.png" width="360" height="350"> 
</p>

- **Average Salary vs. Job Count**: Visualization depicting the average salary for the top 10 categories based on job count and estimated salary.
  
    <img src="https://github.com/ehsanSh21/jobinja-data-modeling/blob/main/avg_estimated.png" width="650" height="400"> 
  
- **Top 10 Skills for Specific Category**: Visualization illustrating the top 10 skills requested for specific job categories, with a slicer for category selection.
<img src="https://github.com/ehsanSh21/jobinja-data-modeling/blob/main/skill_cat.png" width="750" height="400">
<img src="https://github.com/ehsanSh21/jobinja-data-modeling/blob/main/skill_cat_pro.png" width="750" height="400"> 

- **Salary Increase by Experience Years**: Dynamic line chart demonstrating salary increase trends based on experience years for selected categories and provinces.
<img src="https://github.com/ehsanSh21/jobinja-data-modeling/blob/main/experience_cat.png" width="750" height="400">
<img src="https://github.com/ehsanSh21/jobinja-data-modeling/blob/main/experience_cat_prov.png" width="750" height="400">

### Conclusion
The project successfully analyzed the job market data and provided valuable insights into salary trends, skill requirements, and category-specific dynamics.

### References
- jobinja.ir - Data source for job market information.

This version focuses solely on the project details, web scraping process, data preprocessing, data visualization with Power BI, and concluding remarks.
