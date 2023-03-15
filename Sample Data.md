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
	id BIGSERIAL NOT NULL PRIMARY KEY,
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	address VARCHAR(50) NOT NULL,
	county VARCHAR(50) NOT NULL,
	state_code VARCHAR(10) NOT NULL,
	zip_code VARCHAR(10)NOT NULL,
	phone_number VARCHAR(20) NOT NULL,
	email VARCHAR(100) NOT NULL CHECK (email ~* '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$')
)
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
COPY loyalty_programme.customers_info 
FROM 'C:\Users\****** \******\customers_info.csv' DELIMITER ',' CSV; --(file path blanked out for protection)
````
Breaking this command down:

**COPY** - loyalty_programme.customers_info is the schema and table name respectively.

**FROM** – The file specified where the CSV is located.

**DELIMITER** – “,” specifies that a comma delimiter has been used to separate the values in each row

**CSV** – This specifies that the input file format is CSV.

When this command is executed, it reads the data from the CSV file and inserts it into the customers_info table. As the CSV does not have headers, it does not need to be mentioned in the code, though it’s important that data in the different columns in the table align the data in the CSV.

## Data Copied into Table:
![image](https://user-images.githubusercontent.com/121811651/225465204-2a729632-f861-41c7-842b-aeb41daa559c.png)




