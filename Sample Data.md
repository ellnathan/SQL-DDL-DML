# Table of Content

## Create Schema
Before creating a table, it’s good practice to create a schema for the table to be housed in. Though, this is not a necessary requirement for a table as there is a default schema called ‘Public’ available for every new database, creating a schema allows for tables to be organised which is ideal when you have multiple tables or different types of data within table. 

For the purpose of this example, I have a created a new schema called ````loyalty_programme```` as the data in the tables are not related to the hospital sample data that was used in the general_hospital schema. [Click here to see this sample data.](https://github.com/ellnathan/SQL-Hospital-Sample-Data)

The code line to used to create a schema is ```CREATE SCHEMA``` along with the Schema name. Alternatively, the same result can be achieved by right clicking on ```Schemas``` in PostresSQL and selecting Create > Schema. Once the schema has been created, it will appear under the schemas section. 

````sql
CREATE SCHEMA loyalty_programme
````
![create schema](https://user-images.githubusercontent.com/121811651/224580525-8a3dd6d5-bf45-4de9-a0d0-81e8880d573e.png)

## Create Table
How to create a table and decide which data type and constraints to use and why
The syntax for creating tables is similar across most relational database management systems (RDBMS):

````sql
CREATE TABLE table_name (
   column1 datatype1,
   column2 datatype2,
   column3 datatype3,
  )
  ````
  
However, the table that I will be creating will hold customer information, and to ensure I restrict the values that can be inserted or updated, I will need to implement constraints across the different columns to maintain integrity of the data. To create a table with constraints, the syntax the same as above along with the constraint added after the datatype. 
There are many different types of datatypes and constraints that can be defined when creating a table. Below is a link to the find out more information:

**DataTypes in PostgresSQL** - https://www.postgresql.org/docs/current/datatype.html

**Constraints in PostgresSQL** - https://www.tutorialspoint.com/postgresql/postgresql_constraints.htm

```sql
CREATE TABLE loyalty_programme.customers_info (
    customer_id BIGSERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    address VARCHAR(50) NOT NULL,
    county VARCHAR(50) NOT NULL,
    state_code VARCHAR(10) NOT NULL,
    zip_code VARCHAR(10) NOT NULL,
    phone_number VARCHAR(20) NOT NULL,
    email VARCHAR(100) NOT NULL CHECK (email ~* '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$')
);

````

**Datatypes used and why:**

**BIGSERIAL:** The bigserial data type is similar to the serial data type, except that it uses a 64-bit integer instead of a 32-bit integer. This means that it can generate a much larger sequence of unique values and automatically generates a unique sequences of values for column ‘id’ each time a new row is inserted into the table. The values will be integers from 1 and will increment by 1 for each new row.

**VARCHAR:** The purpose of VARCHAR is to provide a more flexible way to store textual data in the database. Unlike fixed-length character datatypes like CHAR. For example, if I was to use CHAR(10) it will use up to 10 bytes of storage space regardless of how many characters are actually stored in the column which is inefficient. However, using VARCHAR, it will only use as much storage space as it needs to store the actual data. If for example, I use VARCHAR () and the string is shorter than 10 characters, it will only use the space needed to store that string.  
Though the zip_code column only contains numeric values, INTEGER is not selected as a data type leading zeros could be lost if this column is an INTEGER, thus using VARCHAR data type is more recommended.


**Constraints used and why:**

**PRIMARY KEY**: The primary key uniquely identifies each row in a table. This is useful when querying data from other tables using JOINs, which can be done on connecting both the Primary and Foreign Key together to establish the relationship between the tables.

**NOT NULL:** The purpose of this table is to store customer data which will require mandatory information to be complete, hence NOT NULL is used which does not allow the fields to be blank.

**CHECK:** Use a constraint that is commonly used for emails to validate that the email follows a specific pattern and eliminate any potential errors or invalid characters. 

Once the table has been created, we can check that the datatypes and constraints have been defined by running a simple SELECT ALL statement.

````sql
SELECT * FROM loyalty_programme.customers_info
````
![output table](https://user-images.githubusercontent.com/121811651/224580953-7f691356-af34-4637-a7b1-243e68631a1f.png)

# Adding Data to the Table
To add records to the table, the values can be inputted using INSERT TO statement or copy data from an external file i.e CSV. Starting with the latter, the syntax to copy data from a CSV file into a table:

````sql
COPY loyalty_programme.customers_info(first_name, last_name, address, county, state_code, zip_code, phone_number, email)
FROM 'C:\Users\*****\*****\sample_data.csv'
WITH CSV; --(file path blanked out for protection)
````
Breaking this command down:

**COPY** - Command is used to load data into the table ```loyalty_programme.customers_info``` The column names for which data is being inserted are specified in parentheses after the table name.

**FROM** - Keyword specifies the source file from which data will be loaded

**WITH** keyword specifies that the source file is in CSV format.

When this command is executed, it reads the data from the CSV file and inserts it into the customers_info table. As the CSV does not have headers, it does not need to be mentioned in the code, though it’s important that data in the different columns in the table align the data in the CSV.

## Data Copied into Table:
In the ```CREATE TABLE``` command the ```customer_id``` column was defined as ```BIGSERIAL```. This is a PostgreSQL data type that is used to define a column that is automatically populated with a unique integer value for each new row that is inserted into the table.

When a new row of data is inserted into the table, the customer_id column will automatically generate a unique integer value. This is because the BIGSERIAL data type that is defined generates the unique integer values. This is useful when you need to ensure that each row has a unique identifier, but you don't want to manually specify the value for each new row. 

![image](https://user-images.githubusercontent.com/121811651/225487194-f8fa7039-261a-46f1-9bd1-f30951ce43c3.png)


## Inserting Records:

To add more records to the customers_info table, the INSERT INTO command can be used. The basic syntax:

````sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3...);
````
The column names must be specify in the same order as they appear in the table, for the values which are to be added to the columns. Each value must be enclosed in parentheses and separated by commas and must match the number of columns in the table.

In this example, I’ve added new records to the table which can be found at the bottom of the table. Customer_ID – 501, 502, 503 and 504 which is automatically added.

```sql
INSERT INTO loyalty_programme.customers_info
(first_name,last_name,address,county,state_code,zip_code,phone_number,email)
VALUES
	('Phineas', 'Ferb', '452 Example Road', 'Atlanta','GA','95321', '332-452-1344', 'phineas.ferb@gmail.com')
	('Bart', 'Simpson', 'Springfield Bound', 'Chicago', 'IL', '67011', '462-722-7034', 'bsimpson@aol.com'),
	('Joey', 'Bavo', '382 Rayman Road', 'Dallas', 'TX', '31268', '342-102-1255', 'joeyb@live.com'),
	('Jessica', 'Emard', '567 Village Court', 'Los Angeles', 'CA', '78489', '914-522-6755', 'jemard@gmail.com');
```
![image](https://user-images.githubusercontent.com/121811651/225788598-55a97049-b3a7-4c3e-a294-076a7450e2ec.png)

# Adding an Additional Constraint

An additional constraint worth adding is ```UNIQUE``` for the email column. This is a beneficial constraint to prevent duplicate email addresses from being inserting into the table which could happen if there are customers with similar names and email formats.

```sql
ALTER TABLE loyalty_programme.customers_info
ADD CONSTRAINT unique_email UNIQUE (email)
````






