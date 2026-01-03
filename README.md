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
- Import the data from the csv files saved on the desktop to each of the tables in MySQL, placing the csv files in the C:\wamp64\tmp directory 



### Part 2: Data Clean-Up 

- Joining data from separate datasets in MySQL: https://www.w3schools.com/sql/sql_join.asp
- I removed some data in the original download file so I would have some blank data to deal with. Look for missing data. 

### Part 3: Data Analysis

### Part 4: Visualisation  


## END  