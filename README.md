# Overview
This project demonstrates my process of migrating data from a MySQL database into PostgreSQL, focusing on creating an efficient, relational database structure in PostgreSQL with Python. The project serves as both a practice of PostgreSQL fundamentals—such as primary and foreign keys, indexing, and data insertion—and an introduction to using Python with psycopg2 to manage database connections and data insertion.

# Technologies and Libraries Used
Python: Primary language for scripting the database connection, data manipulation, and data insertion.
Pandas: To manage and manipulate data from CSV files before insertion.
Psycopg2: A PostgreSQL adapter for Python, used to connect to PostgreSQL and execute SQL queries.
PostgreSQL: Target database system, chosen for its powerful features and compatibility with modern applications.
Jupyter Notebook: Environment for interactive coding, allowing me to document and track each step of the migration process.

# Dataset
I found the dataset used in this project on [Relational Data's Consumer Expenditures Dataset](https://relational-data.org/dataset/ConsumerExpenditures), which contains multiple tables. This allowed me to practice database design and migration across different database management systems.

# Project Steps
## Dataset Acquisition:
I discovered that the dataset source included credentials for accessing a MySQL database, allowing me to retrieve the complete database directly.
Using these credentials, I accessed the full MySQL database, which contained large tables with extensive rows.

##Data Extraction and Sampling:
Given the size of the tables, I opted to extract only the first 500 rows from each of the three key tables: expenditures, household_members, and households.
I saved these samples into CSV files for easier handling and testing in the migration process.

## MySQL DDL Extraction:
To ensure that my new PostgreSQL database would closely mirror the original MySQL schema, I extracted the MySQL Data Definition Language (DDL) statements for each table.
I included screenshots of these DDL statements in the original_mysql_DDL_tables folder, which I referenced when creating the PostgreSQL schema.

## Database Connection and Setup in Python:
Using psycopg2, I connected to my local PostgreSQL instance from within a Jupyter Notebook, where I created a cursor to execute SQL queries.
I started by creating a new PostgreSQL database, followed by tables modeled after the original MySQL schema.

## Table and Schema Configuration:
For each table, I added columns based on the MySQL DDL and configured constraints such as:
Primary Keys: For uniquely identifying rows within each table.
Foreign Keys: To maintain relational integrity across tables.
Indexes: For optimizing query performance.
I also set up default values, NOT NULL constraints, and other necessary configurations for each column.

## Data Loading with Pandas:
I loaded each CSV file into a Pandas DataFrame, preparing it for insertion into the PostgreSQL tables.

## Handling Foreign Key Constraints:
Since I only imported a partial dataset (500 rows) from each table, some rows may reference missing data in other tables. This can cause foreign key errors when inserting data.
Based on project requirements, I decided to remove these rows with missing references in foreign key columns rather than modify the referenced table. This ensures data integrity without introducing incomplete references.
I also made sure to insert rows into referenced tables first (e.g., households) before inserting into referencing tables (e.g., household_members and expenditures) to satisfy foreign key constraints.

## Function-Based Code Structure:
To make this process more reproducible, I organized the code into functions, allowing others to easily follow and reuse the steps by simply calling the respective functions.
Detailed explanations of each step are included within the Jupyter Notebook for additional guidance.


# Lessons Learned and Key Takeaways
This project provided hands-on experience with database migrations, schema design, and indexing strategies in PostgreSQL. Additionally, it reinforced the use of Python libraries like pandas and psycopg2 to facilitate database connections and data management.
