# programming-for-data-analytics-project
This repository ....

This repository implements an automated data acquisition and visualisation workflow for hourly FAANG stock price data using Python and GitHub Actions. Expected outputs are xxx 

## Purpose  
This work aims to demonstrate competencies in computational data acquisition, clean-up, database use, analysis, and visualisation. Specifically, the repository xxxxx.

## Requirements
The code was developed and tested using Python 3.12.1. The following external dependencies are required (see requirements.txt file):   
- matplotlib  
- pandas  
-  
All other modules used are part of the Python standard library.  

To install dependencies, run:  

```bash
# Install dependencies
pip install -r requirements.txt
```  

## Usage  

### 1. Running the script from the terminal  
`

### 2. Running the script in a Jupyter notebook (Python code cell) 



## Project Work 

### Part 1: Data Acquisition and Database Creation

Researching databases which can adequately simulate workplace data, two Kaggle datasets were identified; the (Student Records dataset)[https://www.kaggle.com/datasets/pratikprasad18/student-records] and the (Students Grading dataset)[https://www.kaggle.com/datasets/mahmoudelhemaly/students-grading-dataset].

Reviewing the SQL lectures, completing the (W3 Schools Tutorial)[https://www.w3schools.com/sql/], and researching how to (import Kaggle databases into  MySQL)[https://www.youtube.com/watch?v=MzbtTv1TeKU]. 

The databases were first downloaded to the desktop to examine them, to ensure the student ID column had the same column name in both datasets (one was changed to match the other), to reduce the student records database from 10,000 entries to 5,000 and to examine could the student ID columns be used to match the data. The resulting CSV databases (saved to the desktop) were:  
1. student_records - Column titles: StudentID, Class, Math, Science, English, History, Computer, AttendancePercent,	Total
2. students_performance_dataset - Column titles: StudentID, First_Name, Last_Name, Email, Gender, Age, Department, Midterm_Score, Final_Score, Assignments_Avg, Quizzes_Avg, Participation_Score, Projects_Score, Total_Score, Grade, Study_Hours_per_Week, Extracurricular_Activities, Internet_Access_at_Home, Parent_Education_Level, Family_Income_Level  
Each database was modified to contain 5000 entries in each of the databases.

To import the CSV files into the MySQL (console on the WAMP server).  
- (ChatGPT conversation)[https://chatgpt.com/share/69595ca4-05dc-800d-a103-8a0d245ca9ce] to allow the use of files from a local source before import.  
- (Create a new database)[https://dev.mysql.com/doc/refman/8.4/en/creating-database.html] called `students`.  
- (Create tables)[https://www.w3schools.com/sql/sql_create_table.asp], one for each dataset. Investigating datatypes allowed in (MySQL)[https://www.w3schools.com/sql/sql_datatypes.asp]
- Import the data from the csv files saved on the desktop to each of the tables in MySQL, placing the csv files in the C:\wamp64\tmp directory. However, the data would not import correctly for the students_performance_database csv file as there were duplicate names in the file (although the primary key studentID was unique for each entry), conversation with (ChatGPT)[https://chatgpt.com/share/69595ca4-05dc-800d-a103-8a0d245ca9ce] about this. As there would be no identifying names in my work projects, I removed the `First_Name` and `Last_Name` columns from the corresponding MySQL table (reference)[https://stackoverflow.com/questions/41027336/how-do-i-rename-and-delete-mysql-column-names-without-deleting-rows-or-records], and from the CSV file itself to allow data import into the SQL table.



### Part 2: Data Clean-Up 

- Although no duplicates have been seen or are expected, for good practice, for each table, remove duplicates using "DISTINCT with INTO" MySQL commands to create a temporary table for each original table which has only unique rows in it (thus removing duplicates if they are there), removing the old table and renaming the new table with no duplicates (see reference)[https://www.datacamp.com/tutorial/sql-remove-duplicates?utm_cid=23340058068&utm_aid=192632749329&utm_campaign=230119_1-ps-other%7Edsa-tofu%7Esql_2-b2c_3-emea_4-prc_5-na_6-na_7-le_8-pdsh-go_9-nb-e_10-na_11-na&utm_loc=9040164-&utm_mtd=-c&utm_kw=&utm_source=google&utm_medium=paid_search&utm_content=ps-other%7Eemea-en%7Edsa%7Etofu%7Etutorial%7Esql&gad_source=1&gad_campaignid=23340058068&gbraid=0AAAAADQ9WsErh2BbioEUeZ6_jxSS7siaV&gclid=CjwKCAiAmePKBhAfEiwAU3Ko3L1z-OLmmt7dxD_bsTkVOoXCW0sNXysrxAtZPa7K3hnQ176tJJ0PwBoCsIMQAvD_BwE&dc_referrer=https%3A%2F%2Fwww.google.com%2F] and (ChatGPT conversation)[https://chatgpt.com/share/69597954-7eb4-800d-a62a-db9c9422f5f6]
- Look at the unique and original data tables (reference)[https://dev.mysql.com/doc/refman/8.4/en/describe.html], also, see how many rows of data are in each data tables (stackoverflow)[https://stackoverflow.com/questions/28916917/sql-count-rows-in-a-table] and (documentation)[https://dev.mysql.com/doc/refman/8.4/en/counting-rows.html]. The unique_students_performance_dataset and the unique_student_records tables have the same amount of rows as each of their respective original tables.
- Examining the tables in localhost/phpMyAdmin (username root, password "blank"), the unique data tables are correct, and the originals can be deleted using (DROP TABLE)[https://www.w3schools.com/mysql/mysql_drop_table.asp]. The unique databases are (renamed)[https://www.dbvis.com/thetable/mysql-rename-table-3-different-approaches/#:~:text=Approach%20%231%3A%20Rename%20a%20Table%20With%20ALTER%20TABLE&text=1%20ALTER%20TABLE%20table_name%20RENAME,the%20table_name%20table%20to%20new_table_name%20.] to `student_records_clean` and `students_performance_clean`

- Joining data from separate datasets in MySQL; (W3 schools)[https://www.w3schools.com/sql/sql_join.asp] and (stackoverview)[https://stackoverflow.com/questions/70663741/sql-does-join-create-a-new-table] and (documentation)[https://dev.mysql.com/doc/refman/8.4/en/join.html] and conversation with (ChatGPT)[https://chatgpt.com/share/695988a9-2a48-800d-a42d-5a8ce59013d3] about creating a new table versus modifying an original table.
Following this work,  a new table `merged_students` is created in the `students` MySQL database. I checked this in phpmyadmin and it looks great. I am so relieved and delighted I am able to do this, as I stated it was my primary motivation for joining this course. 

- I removed some data in the original download file so I would have some blank data to deal with. Researching how to deal with missing data in SQL (W3Schools)[https://www.w3schools.com/sql/sql_isnull.asp],and (generally)[https://towardsdatascience.com/why-you-should-handle-missing-data-and-heres-how-to-do-it-270c321a4d6f/] and in conversation with (ChatGPT)[https://chatgpt.com/share/69598ad7-c468-800d-b0b1-6cc128fbf0b7], (also)[https://www.datacamp.com/tutorial/techniques-to-handle-missing-data-values],(also)[https://datascience.stackexchange.com/questions/215/where-in-the-workflow-should-we-deal-with-missing-data], I have decided to deal with null values in my data analysis step using pandas etc. as I have more visualisation of the data and the manipulation processes (although I can see it in phpmyadmin), so I will not dedal with missing values here.

Now the database is ready for use on myphpadmin/MySQL, I will pick up my work in the student_outcomes.ipynb notebook for the analysis and presentation steps.

Attempting to import the SQL database and then table into the Jupyter notebook, I encounter the following error "C:\Users\orlaw\AppData\Local\Temp\ipykernel_1528\1911649809.py:14: UserWarning: pandas only supports SQLAlchemy connectable (engine/connection) or database string URI or sqlite3 DBAPI2 connection. Other DBAPI2 objects are not tested. Please consider using SQLAlchemy.
  df = pd.read_sql(query, conn)"

Therefore, researching SQLAlchemy on (GitHub)[https://github.com/sqlalchemy/sqlalchemy]

I had a huge amount of trouble doing this as it turns out GitHub (linux) does not support the use of MySQL(windows version on the wamp server on my laptop) so after a lot of trouble shooting with (ChatGPT)[https://chatgpt.com/share/6959b211-f014-800d-b3bc-98aaaf991e3e] and (ChatGTP)[https://chatgpt.com/share/6959b22d-b120-800d-ac1a-556280e33075], I had a thought that if I could work on my repo on my local VS Code, this may work (asking (ChatGPT)[https://chatgpt.com/share/6959b19a-10ec-800d-ae43-7af42a2960c2]). So I opened the repo and after git pull, proceeded to import the database table and this worked! Finally!!!!

To make this permanent in my repo, I will save the table to a `.csv.` file at the `root` of the repository.



Importing the SQL database and tables so they can be used in my codespace  ipynb:

All of above conversation: https://chatgpt.com/share/695c51ca-c5d4-800d-91fb-ac7140f42fc6

tidy up code with last bit of code it gave.


### Part 3: Data Analysis

### Part 4: Visualisation  


## END  