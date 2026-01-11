# programming-for-data-analytics-project
This repository contains a data analytics workflow using SQL database management and statistical analysis in Python. Two student datasets ("student data" and "student outcomes data") were acquired from Kaggle, stored in a MySQL database, merged using a shared identifier (StudentID) within the MySQL database, analyzed within a Jupyter notebook using pandas and scikit-learn, and visualized using matplotlib and seaborn. 

## Background
The motivation for undertaking this H.Dip. programme was to acquire the practical skills required to merge datasets using a shared linking key (student ID) and to analyze the resulting integrated 
dataset, using code. In a previous large-scale work-place project, these tasks were conducted by a statistician due to the scale and complexity of the dataset. This project replicates that workflow on a smaller, simulated dataset, enabling practical learning with respect to database design and conducting statistical analysis through Python. 

## Purpose  
The purpose of this project is to simulate a basic student outcomes research study and to demonstrate competency in:  
- Data acquisition and inspection,   
- Relational database design and SQL querying,    
- Dataset integration using a primary key,  
- Data cleaning and validation,  
- Statistical analysis and visualization using Python.  

## Requirements
The code was developed and tested using Python 3.12.1. Required external dependencies are listed in `requirements.txt`:   
- numpy    
- pandas   
- sqlalchemy
- pymysql 
- matplotlib
- seaborn
- scikit-learn
- scipy
- cryptography
All other modules used are part of the Python standard library.  

To install dependencies, run:  

```bash
# Install dependencies
python -m pip install -r requirements.txt
```  

### Running the Analysis
The primary analysis is conduced in the Jupyter notebook `student_outcomes.ipynb`.
To run the notebook:  
1. Ensure all dependencies listed in the `requirements.txt` file are installed in the active environment,    
2. Ensure that the MySQL database is available, either via the Docker container defined in `docker-compose.yml` or an equivalent local MySQL instance containing the `students` database and `merged_students` table,  
3. Launch Jupyter from the repository root: 
```bash
jupyter notebook
```  
4. Open `student_outcomes.ipynb`,    
5. Run all notebook cells sequentially from top to bottom.  

## Repository Structure
- `student_outcomes.ipynb` — main Jupyter notebook,    
- `docker-compose.yml` — MySQL container configuration,   
- `students.sql` — database dump for reproducibility,    
- `requirements.txt` — Python dependencies,   
- `README.md` — project documentation,  
- `clean_merged_students.csv` - saved CSV for permanency,  
- `student_records.csv` - original Kaggle dataset saved for permanence and to aid assessment of the project,  
- `students_performance_dataset.csv` - as above.  

## Data Sources
Two Kaggle datasets were selected to simulate realistic project data: 
- [Student Records dataset](https://www.kaggle.com/datasets/pratikprasad18/student-records), and  
- [Student Performance and Behavior dataset](https://www.kaggle.com/datasets/mahmoudelhemaly/students-grading-dataset). 
To determine the best use of this data, SQL lectures were reviewed, the [W3 Schools Tutorial](https://www.w3schools.com/sql/) was completed, and guidance was consulted on to [import Kaggle databases into  MySQL](https://www.youtube.com/watch?v=MzbtTv1TeKU). 

To aid assessment, these datasets are saved as CSV files at the `root` of the repository.

Both datasets were downloaded locally for inspection to confirm the presence of a common linking variable (`StudentID`). One dataset required the renaming of the identifier column to ensure consistency between the two datasets. The original datasets contained 10,000 entries and 5,000 records, respectively. Both were modified to contain 5000 entries to enable a one-to-one merge (saved to the `root` of the repository for permanence as `student_records.csv` and `students_performance_dataset.csv`).

To ensure exposure to missing-data handling, values were deliberately removed from the `CSV` files prior to import. 

## Database Creation and Data Integration
The `CSV` files were imported into MySQL via the WAMP server console. This process involved:    
- Enabling local file imports following guidance from a [ChatGPT discussion](https://chatgpt.com/share/69595ca4-05dc-800d-a103-8a0d245ca9ce) to allow the use of files from a local source before import,    
-  Creating a new database named `students`, following the [MySQL documentation](https://dev.mysql.com/doc/refman/8.4/en/creating-database.html),     
- Creating separate tables for each dataset using the [CREATE TABLE](https://www.w3schools.com/sql/sql_create_table.asp) command and reviewing the permissible [MySQL data types](https://www.w3schools.com/sql/sql_datatypes.asp).  

During import, the `students_performance_database` `CSV` file failed to load correctly due to duplicate names values, despite the primary key (`StudentID`) being unique. This issue was consulted upon with [ChatGPT](https://chatgpt.com/share/69595ca4-05dc-800d-a103-8a0d245ca9ce). As personally identifying information would not be present in comparable workplace datasets, the `First_Name` and `Last_Name` columns were removed from both the MySQL table and the `CSV` files itself, following guidance from [StackOverflow](https://stackoverflow.com/questions/41027336/how-do-i-rename-and-delete-mysql-column-names-without-deleting-rows-or-records). This modification enabled successful data import. The database was named `Students` and the tables `student_records` and `students_performance_dataset`. 

Duplicate removal was carried out as good practice using `SELECT DISTINCT ... INTO` queries to create temporary tables (`unique_students_performance_dataset` and `unique_student_records`) following the approach outlined by DataCamp and supporting discussion with Chat GPT ([DataCamp](https://www.datacamp.com/tutorial/sql-remove-duplicates?utm_cid=23340058068&utm_aid=192632749329&utm_campaign=230119_1-ps-other%7Edsa-tofu%7Esql_2-b2c_3-emea_4-prc_5-na_6-na_7-le_8-pdsh-go_9-nb-e_10-na_11-na&utm_loc=9040164-&utm_mtd=-c&utm_kw=&utm_source=google&utm_medium=paid_search&utm_content=ps-other%7Eemea-en%7Edsa%7Etofu%7Etutorial%7Esql&gad_source=1&gad_campaignid=23340058068&gbraid=0AAAAADQ9WsErh2BbioEUeZ6_jxSS7siaV&gclid=CjwKCAiAmePKBhAfEiwAU3Ko3L1z-OLmmt7dxD_bsTkVOoXCW0sNXysrxAtZPa7K3hnQ176tJJ0PwBoCsIMQAvD_BwE&dc_referrer=https%3A%2F%2Fwww.google.com%2F), [ChatGPT](https://chatgpt.com/share/69597954-7eb4-800d-a62a-db9c9422f5f6), and [ChatGPT](https://chatgpt.com/share/695c51ca-c5d4-800d-91fb-ac7140f42fc6)).  

The original and de-duplicated tables were examined using `DESCRIBE` ([MySQL documentation](https://dev.mysql.com/doc/refman/8.4/en/describe.html)) and row counts were verified using `COUNT(*)` ([StackOverflow](https://stackoverflow.com/questions/28916917/sql-count-rows-in-a-table), [MySQL documentation](https://dev.mysql.com/doc/refman/8.4/en/counting-rows.html)). The `unique_students_performance_dataset` and the `unique_student_records` tables were confirmed to contain the same number of rows as their respective original tables.  

Inspection in phpMyAdmin, confirmed that the de-duplicated tables were correct. The original tables were removed using `DROP TABLE` ([W3Schools](https://www.w3schools.com/mysql/mysql_drop_table.asp)), and the cleaned tables were renamed to`student_records_clean` and `students_performance_clean`([DBVis](https://www.dbvis.com/thetable/mysql-rename-table-3-different-approaches/#:~:text=Approach%20%231%3A%20Rename%20a%20Table%20With%20ALTER%20TABLE&text=1%20ALTER%20TABLE%20table_name%20RENAME,the%20table_name%20table%20to%20new_table_name%20.))
The cleaned datasets were subsequently joined in the MySQL console using a shared key (`StudentID`), following guidance from W3Schools, Stack Overflow, and the MySQL documentation ([W3 schools](https://www.w3schools.com/sql/sql_join.asp), [Stack Overview](https://stackoverflow.com/questions/70663741/sql-does-join-create-a-new-table), [MySQL documentation](https://dev.mysql.com/doc/refman/8.4/en/join.html)) and informed by discussion with ChatGPT ([ChatGPT](https://chatgpt.com/share/695988a9-2a48-800d-a42d-5a8ce59013d3)). This resulted in the creation of a new table, `merged_students`, within the `students` database, which was verified via phpMyAdmin.    

Approaches to managing missing data were reviewed ([W3Schools](https://www.w3schools.com/sql/sql_isnull.asp),[Towards Data Science](https://towardsdatascience.com/why-you-should-handle-missing-data-and-heres-how-to-do-it-270c321a4d6f/), [DataCamp](https://www.datacamp.com/tutorial/techniques-to-handle-missing-data-values), [Data Science Stack Exchange](https://datascience.stackexchange.com/questions/215/where-in-the-workflow-should-we-deal-with-missing-data)), alongside discussion with ChatGPT ([ChatGPT](https://chatgpt.com/share/69598ad7-c468-800d-b0b1-6cc128fbf0b7)). It was decided to defer treatment of missing values to the analysis stage in Python (pandas), where data inspection and transformation are more transparent. Accordingly, no missing data management was carried out at the SQL stage.  

At this point, the `merged_students` dataset was considered ready for analysis. Work therefore transitioned to the `student_outcomes.ipynb` notebook for data analysis and presentation.  

## Database Access in GitHub Codespaces
During attempts to import the MySQL table into `student_outcomes.ipynb` within GitHub Codespaces, a warning indicated that pandas recommends the use of SQLAlchemy connections rather than direct DBAPI (database API) connections. SQLAlchemy was therefore investigated ([GitHub](https://github.com/sqlalchemy/sqlalchemy)). Significant compatibility issues arose due to the differences between the local Windows-based MySQL (WAMP server) environment and the Linux-based GitHub Codespaces environment. After extensive troubleshooting with ChatGPT ([ChatGPT](https://chatgpt.com/share/6959b211-f014-800d-b3bc-98aaaf991e3e), [ChatGPT](https://chatgpt.com/share/6959b22d-b120-800d-ac1a-556280e33075)), the workflow was migrated to a local VS Code environment linked to the repository ([ChatGPT](https://chatgpt.com/share/6959b19a-10ec-800d-ae43-7af42a2960c2)) in an attempt to rectify the issue ([SQLAlchemy documentation](https://docs.sqlalchemy.org/en/20/core/engines.html)). Following a `git pull`, the database table was successfully imported into the notebook. The imported data were saved as a `.CSV.` file at the `root` of the repository for reproducibility (this `CSV` file was later deleted when it was determined this was not the correct course of action).

However, this approach did not resolve the issue, as the local MySQL database remained inaccessible from the GitHub Codespace, even when using SQLAlchemy. Consequently, further troubleshooting was undertaken with ChatGPT ([ChatGPT](https://chatgpt.com/share/6962c51c-e530-800d-9c66-fa653c976f01)), leading to the adoption of a "container" solution. Specifically, a MySQL service was launced using Docker Compose to proved an isolated database ebnvironment within the GitHub Codespace. A `docker-compose.yml` file was created at the `root` of the repository to support this.

The `students` database was exported from the local WAMP server as a `students.sql` dump  using `mysqldump` and then manually transferred to the repository root within the codespace. A `students` database was created within the MySQL container, and privileges were granted to a non-root user (`analyst`) to enforce the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege). The `students.sql` dump was then imported into the container. This included the `merged_students` table required for analysis.  

Finally, the required Python packages (`SQAlchemy`, `PyMySQL`, `cryptography`, and `pandas`) were installed within the Jupyter notebook. A SQLAlchemy engine was used to connect to the MySQL containers, and successful import was validated by loading the `merged_students` table into a pandas DataFrame, and inspected. 

At this stage, the `merged_students` dataset was accessible and ready for analysis. 

## Data Analysis  
The following analyses were carried out within `student_outcomes.ipynb`:   

a) Descriptive statistics  
Initial inspection summarized demographic characteristics and academic performance. Counts ([W3Schools](https://www.w3schools.com/sql/func_mysql_count.asp)), means ([W3Schools](https://www.w3schools.com/sql/sql_avg.asp)), and summary statistics were computed using SQL for validation and pandas for grouped summaries by gender and department. The dataset (n=5,000) showed a balanced gender distribution, and a mean age of approximately 21 years. Average total scores and attendance rates were broadly similar across demographic groups.
    
b) Correlation Analysis  
Pearson correlation coefficients (Chat with [ChatGPT](https://chatgpt.com/share/695da3af-171c-800d-9e02-85f9fd44ea6a), and [ChatGPT](https://chatgpt.com/share/6962ef88-9724-800d-b3f8-5938323c5e0e)) were used to assess linear relationships between attendance, assessment components, and total score. Results indicated negligible correlation between attendance and total score, while midterm and project scores showed moderate positive associations, suggesting that continuous assessment components are more informative indicators of overall performance. 

c) Multiple Linear Regression  
A multiple linear regression model was fitted with total score as the dependent variable and academic scores, attendance, age, gender, and department as predictors. Categorical variables were encoded using [one-hot coding](https://www.datacamp.com/tutorial/one-hot-encoding-python-tutorial?utm_cid=19589720821&utm_aid=157098104375&utm_campaign=230119_1-ps-other~dsa-tofu~all_2-b2c_3-emea_4-prc_5-na_6-na_7-le_8-pdsh-go_9-nb-e_10-na_11-na&utm_loc=9040164-&utm_mtd=-c&utm_kw=&utm_source=google&utm_medium=paid_search&utm_content=ps-other~emea-en~dsa~tofu~tutorial~artificial-intelligence&gad_source=1&gad_campaignid=19589720821&gbraid=0AAAAADQ9WsFR5FcpfkAwKRf3fOdzCKvW7&gclid=Cj0KCQiAsY3LBhCwARIsAF6O6XiqhR6O4stLlvxjhoqBSmOH-hOaArNkRw4MWhJ1FHjATEaqXwDjNegaAp0_EALw_wcB). Midterm and project scores emerged as the strongest predictors of total score. Attendance and demographic variables contributed minimally once academic performance measures were included. 

d) Analysis of Variance    
A one-way ANOVA was used to test for differences in mean total scores across academic departments. AAlthough statistically significant, effect sizes were small. Boxplots illustrated substantial overlap between departmetns, indicating that departmental affiliation explains only a limited proportion of outcome variability. Statistical options discussion with ([ChatGPT](https://chatgpt.com/share/695da4ed-0630-800d-8cf3-a2b2365f7869)).

e) Principal Component Analysis  
PCA ([DataCamp](https://www.datacamp.com/tutorial/principal-component-analysis-in-python?utm_cid=23340058065&utm_aid=192632748929&utm_campaign=230119_1-ps-other~dsa-tofu~python_2-b2c_3-emea_4-prc_5-na_6-na_7-le_8-pdsh-go_9-nb-e_10-na_11-na&utm_loc=9040164-&utm_mtd=-c&utm_kw=&utm_source=google&utm_medium=paid_search&utm_content=ps-other~emea-en~dsa~tofu~tutorial~python&gad_source=1&gad_campaignid=23340058065&gbraid=0AAAAADQ9WsEYaLdoyVm7Vb7HFE41zrlnD&gclid=Cj0KCQiAsY3LBhCwARIsAF6O6XizHCoetwXd2SpnGMcVOr1u9unzFTim6ntLi7eRbGNj4iO4JAwDxEgaAmlCEALw_wcB, [Towards Data Science](https://towardsdatascience.com/dive-into-pca-principal-component-analysis-with-python-43ded13ead21/),  [Built In](https://builtin.com/data-science/step-step-explanation-principal-component-analysis) and [Geeks for Geeks](https://www.geeksforgeeks.org/data-analysis/principal-component-analysis-pca/)) was applied as it had been in the work-place project. It was applied here to standardized academic performance variables to investigate whether multiple assessment measures could be summarized by a smaller number of latent dimensions. A scree plot of cumulative explained variance indicated that the first principal component captured a substantial proportion of variance. Loadings showed that this component reflected continuous assessment performance (midterm and project scores), supporting the use of total score as a valid summary measure, while highlighting the multidimensional structure of academic performance.

## Conclusions  
This analysis demonstrates the integration of multiple student datasets to explore predictors of academic performance. Results indicate that for this dataset, continuous assessment academic performance (project and midterm scores) is the primary determinant of overall student outcomes (i.e. total score).  
Completion of this project was challenging, particularly with respect to MySQL configuration and cross-environment compatibility. However, the resulting workflow closely mirrors the previous large-scale work-place project and demonstrates that a similar analysis could now be conducted solo and independently using reproducible, code-based methods. The upcoming databases module is greatly anticipated.

### END  