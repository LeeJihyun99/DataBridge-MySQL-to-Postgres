# Overview
This project involves transferring a MySQL dataset into a PostgreSQL database using Python and psycopg2. The dataset contains multiple tables, and I connected to a local PostgreSQL instance to re-create the tables, configure their constraints, and insert the data from the original CSV files. The project also includes detailed functions with docstrings to facilitate understanding and reusability.

# Project Structure
The repository contains the following folders and files:
```
/project_directory
├── /ddl_info
│   ├── expenditures_DDL.png   # DDL screenshot of expenditures table from MySQL
│   ├── household_members_DDL.png  # DDL screenshot of household_members table from MySQL
│   ├── households_DDL.png     # DDL screenshot of households table from MySQL
├── /final_data
│   ├── households_final_data.csv         # 500 rows after insertion into PostgreSQL
│   ├── expenditures_final_data.csv       # 9 rows after insertion into PostgreSQL
│   ├── household_members_final_data.csv  # 292 rows after insertion into PostgreSQL
├── /original_data
│   ├── expenditures.csv       # 500 rows from the expenditures table in MySQL
│   ├── household_members.csv  # 500 rows from the household_members table in MySQL
│   ├── households.csv         # 500 rows from the households table in MySQL
├── Database_Schema_Diagram    # The schema diagram of the PostgreSQL database
├── README.md                  # Project description and details (this file)
└── project.ipynb              # Jupyter Notebook containing code and function descriptions            
```
# Detailed Description

## 1. Database Schema Diagram
The diagram below represents the schema of the PostgreSQL database after creating the tables and inserting the data. The diagram shows the tables (households, expenditures, household_members), their columns, data types, primary keys, and foreign key relationships.


## 2. Database DDL Information
In the /ddl_info folder, you will find screenshots of the original MySQL DDLs (Data Definition Language) for all three tables: households, expenditures, and household_members. These images represent the structure of the original tables from the MySQL database, which served as the basis for recreating them in PostgreSQL.

## 3. Data Folders
Original Data: The /original_data folder contains 500 rows from each of the three tables (households, expenditures, and household_members) extracted from MySQL and saved as CSV files.

Final Data: The /final_data folder contains the data after it has been inserted into PostgreSQL. The data is now in the form of 500 rows in the households table, 9 rows in the expenditures table, and 292 rows in the household_members table.

## 4. Functions and Docstrings
The following functions are implemented in the Jupyter notebook to help automate the database schema setup and data insertion processes. Each function includes a detailed docstring for clarity. You can view the documentation for each function by calling help(function_name) in a Jupyter notebook cell.

# Technologies and Libraries Used
#### Python: Primary language for scripting the database connection, data manipulation, and data insertion.
#### Pandas: To manage and manipulate data from CSV files before insertion.
#### Psycopg2: A PostgreSQL adapter for Python, used to connect to PostgreSQL and execute SQL queries.
#### PostgreSQL: Target database system, chosen for its powerful features and compatibility with modern applications.
#### Jupyter Notebook: Environment for interactive coding, allowing me to document and track each step of the migration process.

# Dataset
I found the dataset used in this project on [Relational Data's Consumer Expenditures Dataset](https://relational-data.org/dataset/ConsumerExpenditures), which contains multiple tables. This allowed me to practice database design and migration across different database management systems.
The dataset I used is from [this](https://relational.fel.cvut.cz/dataset/ConsumerExpenditures).

# Project Steps
### Dataset Acquisition:
I discovered that the dataset source included credentials for accessing a MySQL database, allowing me to retrieve the complete database directly.
Using these credentials, I accessed the full MySQL database, which contained large tables with extensive rows.

### Data Extraction and Sampling:
Given the size of the tables, I opted to extract only the first 500 rows from each of the three key tables: expenditures, household_members, and households.
I saved these samples into CSV files for easier handling and testing in the migration process.

### MySQL DDL Extraction:
To ensure that my new PostgreSQL database would closely mirror the original MySQL schema, I extracted the MySQL Data Definition Language (DDL) statements for each table.
I included screenshots of these DDL statements in the original_mysql_DDL_tables folder, which I referenced when creating the PostgreSQL schema.

### Database Connection and Setup in Python:
Using psycopg2, I connected to my local PostgreSQL instance from within a Jupyter Notebook, where I created a cursor to execute SQL queries.
I started by creating a new PostgreSQL database, followed by tables modeled after the original MySQL schema.

### Table and Schema Configuration:
For each table, I added columns based on the MySQL DDL and configured constraints such as:
Primary Keys: For uniquely identifying rows within each table.
Foreign Keys: To maintain relational integrity across tables.
Indexes: For optimizing query performance.
I also set up default values, NOT NULL constraints, and other necessary configurations for each column.

### Data Loading with Pandas:
I loaded each CSV file into a Pandas DataFrame, preparing it for insertion into the PostgreSQL tables.

### Handling Foreign Key Constraints:
Since I only imported a partial dataset (500 rows) from each table, some rows may reference missing data in other tables. This can cause foreign key errors when inserting data.
Based on project requirements, I decided to remove these rows with missing references in foreign key columns rather than modify the referenced table. This ensures data integrity without introducing incomplete references.
I also made sure to insert rows into referenced tables first (e.g., households) before inserting into referencing tables (e.g., household_members and expenditures) to satisfy foreign key constraints.

### Migration of all data from three tables 
But if you want to migrate all data from all three tables succesfully, then you need to write queries again and do some modifications in python script too so that you have only the HOUSEHOLD_ID values in the other two referencing tables(EXPENDITURES, HOUSEHOLD_MEMBERS) that are refereced from the HOUSEHOLD table. 
```sql
SELECT * from HOUSEHOLDS ORDER BY HOUSEHOLD_ID LIMIT 500;

WITH HOUSEHOLD AS (
select HOUSEHOLD_ID from HOUSEHOLDS ORDER BY HOUSEHOLD_ID LIMIT 500)
SELECT * FROM EXPENDITURES WHERE HOUSEHOLD_ID IN (SELECT HOUSEHOLD_ID FROM HOUSEHOLD) LIMIT 500;

WITH HOUSEHOLD AS (
select HOUSEHOLD_ID from HOUSEHOLDS ORDER BY HOUSEHOLD_ID LIMIT 500)
SELECT * FROM HOUSEHOLD_MEMBERS WHERE HOUSEHOLD_ID IN (SELECT HOUSEHOLD_ID FROM HOUSEHOLD) LIMIT 500;
```
### Function-Based Code Structure:
To make this process more reproducible, I organized the code into functions, allowing others to easily follow and reuse the steps by simply calling the respective functions.
Detailed explanations of each step are included within the Jupyter Notebook for additional guidance.

#### (Please let me know if there is any mistake in the code or any question about the project! :) )
