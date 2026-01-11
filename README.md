# programming-for-data-analytics-project
This repository contains a data analytics workflow using SQL databases and Python. Two student datasets (student data and student outcomes data) are acquired from Kaggle, merged using a shared identifier (StudentID), stored in a MySQL database, pulled into a Jupyter notebook and analyzed and results plotted within the Jupyter notebook using Python. 

## Purpose  
The purpose of this project is to demonstrate competencies in:  
- Data acquisition,   
- SQL database design and querying,    
- Data merging/integration,  
- Data cleaning and validation of the dataset,  
- Statistical analysis and visualization using Python.  

## Requirements
The code was developed and tested using Python 3.12.1. The following external dependencies are required (see requirements.txt file):   
- numpy    
- pandas   
- mysql-connector-python   
- sqlalchemy  
- pymysql  

All other modules used are part of the Python standard library.  

To install dependencies, run:  

```bash
# Install dependencies
python -m pip install -r requirements.txt
```  

## Usage  

### 2. Running the script in a Jupyter notebook (Python code cell) 



## Project Work 

### Background  
The motivating factor for undertaking this H.Dip. programme was to acquire the skills required to merge datasets using a shared linking key (student ID) and to analyze the resulting integrated dataset, using coding. This objective arose from observing the work of a statistician who was employed to conduct these tasks in a previous large-scale work project, prompting a strong desire to develop the capacity to do this work.   
Accordingly, this this project is designed to simulate a basic student outcomes research study, mimicking the previous project. It encompasses an initial/first use of an SQL database for data storage, the integration of datasets via a key identified data, data-clean up, and subsequent analysis and presentation using Python packages within a Jupyter notebook environment. 


### Part 1: Data Acquisition and Database Creation

Two Kaggle datasets which could adequately similuate project data, were selected: the [Student Records dataset](https://www.kaggle.com/datasets/pratikprasad18/student-records) and the [Student Performance and Behavior dataset](https://www.kaggle.com/datasets/mahmoudelhemaly/students-grading-dataset). To aid assessment, these datasets are saved as CSV files in the repository.

To determine the best use of this data, SQL lectures were reviewed, the [W3 Schools Tutorial](https://www.w3schools.com/sql/) was completed, and guidance was consulted on to [import Kaggle databases into  MySQL](https://www.youtube.com/watch?v=MzbtTv1TeKU). 

Both datasets were downloaded locally for inspection. This allowed verification that a common linking variable (Student ID) could be used for data integration. One dataset required the renaming of the Student ID column to ensure consistency between the two datasets. The Student Records dataset contained 10,000 entries, while the Student Performance and Behavior dataset had 5,000 entries. Therefore, each dataset was modified to contain 5000 entries.
The resulting `CSV` files were:  
1. student_records - Column titles: StudentID, Class, Math, Science, English, History, Computer, AttendancePercent,	Total
2. students_performance_dataset - Column titles: StudentID, First_Name, Last_Name, Email, Gender, Age, Department, Midterm_Score, Final_Score, Assignments_Avg, Quizzes_Avg, Participation_Score, Projects_Score, Total_Score, Grade, Study_Hours_per_Week, Extracurricular_Activities, Internet_Access_at_Home, Parent_Education_Level, Family_Income_Level   

To ensure exposure to missing data handling, as there were no missing values in the datasets, selected values were deliberately removed from the CSV files  I removed some data in the original  `CSV` files prior to import. 

The `CSV` files were imported into MySQL via the WAMP server console. This process involved:    
- Enabling local file imports following guidance from a (ChatGPT discussion)[https://chatgpt.com/share/69595ca4-05dc-800d-a103-8a0d245ca9ce] to allow the use of files from a local source before import,    
-  Creating a new database named `students`, following the (MySQL documentation)[https://dev.mysql.com/doc/refman/8.4/en/creating-database.html],     
- Creating separate tables for each dataset uing the (CREATE TABLE)[https://www.w3schools.com/sql/sql_create_table.asp] command and reviewing the permissible (MySQL data types)[https://www.w3schools.com/sql/sql_datatypes.asp].  
During import, the `students_performance_database` `CSV` file failed to load correctly due to duplicate names values, despite the primary key (StudentID) being unique. This issues was consulted upon with (ChatGPT)[https://chatgpt.com/share/69595ca4-05dc-800d-a103-8a0d245ca9ce]. As personally identifying information would not be present in comparable workplace datasets, the `First_Name` and `Last_Name` columns were removed from both the MySQL table and the `CSV` files itself, following guidance from (StackOverflow)[https://stackoverflow.com/questions/41027336/how-do-i-rename-and-delete-mysql-column-names-without-deleting-rows-or-records],. This modification enabled successful data import. 

The database was named `Students` and the tables `student_records` and `students_performance_dataset`. 

### Part 2: Data Clean-Up and Getting the Data into the GitHub Codespace

Although no duplicates records were observed or expected, duplicate removal was undertaken for good practice. For each table, duplicates were removed using `SELECT DISTINCT ... INTO` MySQL commands to create temporary tables (`unique_students_performance_dataset` and `unique_student_records`) following the approach outlined by DataCamp and supporting discussion with Chat GPT ((DataCamp)[https://www.datacamp.com/tutorial/sql-remove-duplicates?utm_cid=23340058068&utm_aid=192632749329&utm_campaign=230119_1-ps-other%7Edsa-tofu%7Esql_2-b2c_3-emea_4-prc_5-na_6-na_7-le_8-pdsh-go_9-nb-e_10-na_11-na&utm_loc=9040164-&utm_mtd=-c&utm_kw=&utm_source=google&utm_medium=paid_search&utm_content=ps-other%7Eemea-en%7Edsa%7Etofu%7Etutorial%7Esql&gad_source=1&gad_campaignid=23340058068&gbraid=0AAAAADQ9WsErh2BbioEUeZ6_jxSS7siaV&gclid=CjwKCAiAmePKBhAfEiwAU3Ko3L1z-OLmmt7dxD_bsTkVOoXCW0sNXysrxAtZPa7K3hnQ176tJJ0PwBoCsIMQAvD_BwE&dc_referrer=https%3A%2F%2Fwww.google.com%2F] and (ChatGPT)[https://chatgpt.com/share/69597954-7eb4-800d-a62a-db9c9422f5f6]).   

The original and de-duplicated tables were examined using `DESCRIBE` ((MySQL documentation)[https://dev.mysql.com/doc/refman/8.4/en/describe.html]) and row counts were verified using `COUNT(*)` ((StackOverflow)[https://stackoverflow.com/questions/28916917/sql-count-rows-in-a-table], (MySQL documentation)[https://dev.mysql.com/doc/refman/8.4/en/counting-rows.html]). The `unique_students_performance_dataset` and the `unique_student_records` tables were confirmed to contain the same number of rows as their respective original tables.  

Inspection in localhost/phpMyAdmin, confirmed that the de-duplicated tables were correct. The original tables were therefore removed using `DROP TABLE` ((W3Schools)[https://www.w3schools.com/mysql/mysql_drop_table.asp]), and the cleaned tables were renamed to`student_records_clean` and `students_performance_clean`((DBVis)[https://www.dbvis.com/thetable/mysql-rename-table-3-different-approaches/#:~:text=Approach%20%231%3A%20Rename%20a%20Table%20With%20ALTER%20TABLE&text=1%20ALTER%20TABLE%20table_name%20RENAME,the%20table_name%20table%20to%20new_table_name%20.]).

The cleaned datasets were subsequently joined in the MySQL console using a shared key (StudentID), following guidance from W3Schools, Stack Overflow, and the MySQL documentation ((W3 schools)[https://www.w3schools.com/sql/sql_join.asp], (Stack Overview)[https://stackoverflow.com/questions/70663741/sql-does-join-create-a-new-table], (MySQL documentation)[https://dev.mysql.com/doc/refman/8.4/en/join.html]) and informed by discussion with ChatGPT ((ChatGPT)[https://chatgpt.com/share/695988a9-2a48-800d-a42d-5a8ce59013d3]). This resulted in the creation of a new table, `merged_students`, within the `students` database, verified via phpMyAdmin. This step fulfilled the primary motivating objective for enrolment in the programme! :D   

Approaches to managing missing data were reviewed ((W3Schools)[https://www.w3schools.com/sql/sql_isnull.asp],(Towards Data Science)[https://towardsdatascience.com/why-you-should-handle-missing-data-and-heres-how-to-do-it-270c321a4d6f/], (DataCamp)[https://www.datacamp.com/tutorial/techniques-to-handle-missing-data-values], (Data Science Stack Exchange)[https://datascience.stackexchange.com/questions/215/where-in-the-workflow-should-we-deal-with-missing-data]), alongside discussion with ChatGPT ((ChatGPT)[https://chatgpt.com/share/69598ad7-c468-800d-b0b1-6cc128fbf0b7]). It was decided to defer treatment of missing values to the analysis stage in Python (pandas), where data inspection and transformation are more transparent. Accordingly, missing data management was carried out at the SQL stage.  

At this point, the `merged_students` dataset was considered ready for analysis. Work therefore transitioned to the `student_outcomes.ipynb` notebook for data analysis and presentation.

During attempts to import the MySQL table into a Jupyter notebook (`student_outcomes.ipynb`) within GitHub Codespaces, a warning indicated that pandas recommends the use of SQLAlchemy connections rather than direct DBAPI (database API) connections. SQLAlchemy was therefore investigated ((GitHub)[https://github.com/sqlalchemy/sqlalchemy]). Significant compatibility issues arose due to the differences between the local Windows-based MySQL (WAMP server) environment and the Linux-based GitHub Codespaces environment. After extensive troubleshooting with ChatGPT ((ChatGPT)[https://chatgpt.com/share/6959b211-f014-800d-b3bc-98aaaf991e3e], (ChatGPT)[https://chatgpt.com/share/6959b22d-b120-800d-ac1a-556280e33075]), the workflow was migrated to a local VS Code environment linked to the repository ((ChatGPT)[https://chatgpt.com/share/6959b19a-10ec-800d-ae43-7af42a2960c2]) in an attempt to rectify the issue. Following a `git pull`, the database table was successfully imported into the notebook. The imported data were saved as a `.CSV.` file at the `root` of the repository for reproducibility. This CSV file was later deleted when it was determined this was not the correct course of action.

However, this approach did not resolve the issue, as the local MySQL database remained inaccessible from the GitHub Codespace, even when using SQLAlchemy. Consequently, further troubleshooting was undertaken with ChatGPT ((ChatGPT)[https://chatgpt.com/share/6962c51c-e530-800d-9c66-fa653c976f01]), leading to the adoption of a "container" solution. Specifically, a MySQL service was launced using Docker Compose to proved an isolated database ebnvironment within the GitHub Codespace. A `docker-compose.yml` file was created at the `root` of the repository to support this.

The `students` database was exported from the local WAMP server as a `students.sql` dump  using `mysqldump` and then manually transferred to the repository root within the codespace. A `students` database was created within the MySQL container, and privileges were granted to a non-root user (`analyst`) to enforce the principle of least privilege (). The `students.sql` dump was then imported into the container, including the `merged_students` table required for analysis.  

Finally, the required Python packages (`SQAlchemy`, `PyMySQL`, `cryptography`, and `pandas`) were installed within the Jupyter notebook. A SQLAlchemy engine was used to connnect to the MySQL containers, and successful import was validated by loading the `merged_students` table into a pandas DataFrame, and inspected using the `head()` function. 

At this stage, the `merged_students` dataset was accessible and ready for analysis. 

### Part 3: Data Analysis

The following analysis was carried out using MySQL commands within the MySQL container in the Codespace:

a) Descriptive statistics 
Initial database investigations and determination of basic measures (as well as references listed below, co-pilot assisted while coding):   

- The total number of students in the dataset ((W3Schools)[https://www.w3schools.com/sql/func_mysql_count.asp]),   
- The number of males and females in the dataset ((Stack Overflow)[https://stackoverflow.com/questions/17119151/count-male-female-and-total]),    
- The average age of all students ((W3Schools)[https://www.w3schools.com/sql/sql_avg.asp]),  
- The average age for males and females,  
- The average Total Score,   
- The average Total Score for males and for females,  
- The sum of students in each department,    
- Average attendance for all students,  
- Average attendance for males and females,



b) Determining relationships between variables  
- Investigation for relationships between variables (including, attendance, sex, project score, midterm score) and final scorem  

c) Exploring Variance
- MANOVA

### Part 4: Visualisation  


## END  