# Create Schema
```sql
CREATE SCHEMA loyalty_programme
```
# Create Table
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
    email VARCHAR(100) NOT NULL CHECK (email ~* '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$');
```
# CSV File Import
```sql
COPY loyalty_programme.customers_info(first_name, last_name, address, county, state_code, zip_code, phone_number, email)
FROM 'C:\Users\*****\*****\sample_data.csv'
WITH CSV; --(file path blanked out for protection)
```
# Insert
```sql
INSERT INTO loyalty_programme.customers_info
(first_name,last_name,address,county,state_code,zip_code,phone_number,email)
VALUES
	('Phineas', 'Ferb', '452 Example Road', 'Atlanta','GA','95321', '332-452-1344', 'phineas.ferb@gmail.com')
	('Bart', 'Simpson', 'Springfield Bound', 'Chicago', 'IL', '67011', '462-722-7034', 'bsimpson@aol.com'),
	('Joey', 'Bavo', '382 Rayman Road', 'Dallas', 'TX', '31268', '342-102-1255', 'joeyb@live.com'),
	('Jessica', 'Emard', '567 Village Court', 'Los Angeles', 'CA', '78489', '914-522-6755', 'jemard@gmail.com');
```
# Constraints
```sql
ALTER TABLE loyalty_programme.customers_info
ADD CONSTRAINT unique_email UNIQUE (email)
```
# Update
```sql
SELECT *
FROM loyalty_programme.customers_info
WHERE COUNTY IN ('Orleans', 'Baltimore City')
ORDER BY COUNTY
```
# CASE
```sql
UPDATE loyalty_programme.customers_info
SET county = CASE 
              WHEN county = 'Baltimore City' THEN 'Baltimore'
              WHEN county = 'Orleans' THEN 'New Orleans'
              ELSE county
            END
```
