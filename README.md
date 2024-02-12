# Analyzing Job Market Data in Power BI

### Project Overview
This project aims to analyze the job market data obtained from jobinja.ir using web scraping techniques and visualize the insights using Power BI.

### Web Scraping

The project utilized Python along with SQLAlchemy and SQLite3 to scrape job market data from jobinja.ir, resulting in the collection of information on approximately 18000 job listings across the country.

#### Steps Taken
1. **Web Scraping Process**: Employed Python scripts to scrape job data from jobinja.ir.
2. **Data Collection**: Gathered job information, including title, company ID, category, and salary (where available).
3. **Data Storage**: Utilized SQLite3 database with SQLAlchemy ORM for efficient data storage.

### Data Preprocessing

To address the issue of a significant portion of job listings lacking salary information, an estimation method was devised to approximate salaries based on experience years, job category, and province.

#### Methodology
1. **Salary Estimation**: Estimated salaries for jobs with no specified value by analyzing experience years, job category, and province.
2. **Data Augmentation**: Introduced a new column named "estimated_salary" to incorporate the estimated salary values into the dataset.

### Data Model
The data model represents the schema used for storing job market data in the SQLite3 database. It consists of four main entities: Jobs, Skills, Companies, and Job_Skills, with defined relationships between them.
#### Diagram
[Insert Data Model Diagram Here]

### Data Visualization with Power BI
#### Overview
Power BI was employed to visualize and analyze the collected job market data, providing interactive and insightful visualizations.

#### Visualizations
- **Average Salary by Top 10 Categories**: Dynamic visualization showcasing the average salary for the top 10 categories, with a slicer for selecting specific provinces.
- **Average Salary vs. Job Count**: Visualization depicting the average salary for the top 10 categories based on job count and estimated salary.
- **Top 10 Skills for Specific Category**: Visualization illustrating the top 10 skills requested for specific job categories, with a slicer for category selection.
- **Salary Increase by Experience Years**: Dynamic line chart demonstrating salary increase trends based on experience years for selected categories and provinces.

### Conclusion
The project successfully analyzed the job market data and provided valuable insights into salary trends, skill requirements, and category-specific dynamics.

### References
- jobinja.ir - Data source for job market information.

This version focuses solely on the project details, web scraping process, data preprocessing, data visualization with Power BI, and concluding remarks.
