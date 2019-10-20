## Clear Screen
```
\! cls 
```

## List Databases
```
\l
```

## Connect Database from cmd
```
psql -h localhost -U gokhan -p 5432 test
psql host 'address_here' -U 'user_here' -p 'port_here' 'database_name'
```

## Connect from Psql CLI
```
\c database_name
```

## Create Table
```sql
CREATE TABLE person (
    id int, 
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    gender VARCHAR(6),
    date_of_birth DATE,
);
```

## List All Tables
```
\d
\d `table_name`
```

## List Only Created Tables
```
\dt
```

## Table Creation with Constraints
```sql
CREATE TABLE person (
    id BIGSERIAL NOT NULL PRIMARY KEY, 
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    gender VARCHAR(6) NOT NULL,
    date_of_birth DATE NOT NULL,
    email VARCHAR(150)
);
```

## Insert Records
```sql
INSERT INTO person (
    first_name,
    last_name,
    gender,
    date_of_birth,
    email 
)
VALUES (
    'Gokhan',
    'KOC',
    'MALE',
    '1989-03-31',
    'info@gokhankoc.pl'
);
```

## Execute Commands From File
```
\i 'FILE_NAME'
```

## Add Column
```sql
ALTER TABLE person
ADD COLUMN country_of_birth VARCHAR(50) NOT NULL
```

## Remove Column
_CASCADE removes all dependancies_  
_IF EXISTS is optional_  
```sql
ALTER TABLE person
DROP COLUMN 
IF EXISTS
country_of_birth
```

## Ordering
```sql
SELECT * FROM person
ORDER BY date_of_birth 
ASC;
```

## Removing Duplicates with 'DISTINCT'
```sql
SELECT DISTINCT
country_of_birth FROM person;
```

## Filter with 'WHERE' , 'AND'
```sql
SELECT * FROM person 
WHERE gender = 'Male' AND date_of_birth > '2018-12-30';
```

## Comparison Operators ( = , <= , => , <> )
_Returns True (t) or False (f)_
```sql
SELECT 1 = 1;
```
```sql
SELECT 1 <> 1;
```

## Limit, Offset, Fetch
_LIMIT gets first 10_
```sql
SELECT * FROM person 
LIMIT 10;
```
_OFFSET offsets first 10 items_
```sql
SELECT * FROM person 
OFFSET 10;
```
_FETCH is the default way of limiting in SQL standard_
```sql
SELECT * FROM person 
FETCH FIRST 10 ROW ONLY;
```

## 'IN' Operator
_Use Paranthesis to define an array, ex: ('Hello','World')_
```sql
SELECT * FROM person 
WHERE country_of_birth 
IN ('China','Brazil','France');
```

## 'BETWEEN' Operator
_Use AND operator_
```sql
SELECT * FROM person 
WHERE date_of_birth 
BETWEEN '2000-01-01' AND '2016-01-01';
```

## 'LIKE' Operator
_Underscore sign _ stands for single character_  
_Percentage sign % stands for any character before or after_   
_ILIKE ignores cases_
```sql
SELECT * FROM person 
WHERE email 
LIKE '%bloomberg%';
```

## Grouping
```sql
SELECT country_of_birth, COUNT(*) FROM person 
GROUP BY country_of_birth
```

## 'HAVING' Operator
```sql
SELECT country_of_birth, COUNT(*) FROM person 
GROUP BY country_of_birth
HAVING COUNT(*) > 50;
```

## 'COALESCE' Function
_Returns a default value_
```sql
SELECT COALESCE(email, 'email not provided') FROM person;
```

## 'NULLIF' Function
_Returns null if both values are equal_
```sql
SELECT NULLIF(email, 'info@gokhankoc.pl') FROM person;
```

## Extracting Fields From TIMESTAMP
```sql
SELECT EXTRACT( YEAR FROM NOW() );
```
```sql
SELECT EXTRACT( MONTH FROM NOW() );
```
```sql
SELECT EXTRACT( DAY FROM NOW() );
```

## AGE Function
_Gets difference between two dates_
```sql
SELECT AGE(NOW(), date_of_birth) FROM person;
```

## Adding Primary Key
_Values should be unique_
```sql
ALTER TABLE person ADD PRIMARY KEY (id);
```

## Deleting Records
```sql
DELETE FROM person WHERE ID = 2;
```

## Deleting All Records!!
```sql
DELETE FROM person;
```

## Updating Records
```sql
UPDATE person SET email='info@gokhankoc.pl' WHERE ID = 2;
```

## Error Handling
_ON CONFLICT DO NOTHING_
```sql
INSERT INTO person (id, first_name) VALUES (1, 'gokhan') ON CONFLICT (id) DO NOTHING;
```

_ON CONFLICT DO UPDATE_
```sql
INSERT INTO person (id, first_name) VALUES (1, 'gokhan') ON CONFLICT (id) DO UPDATE email='gokhankoc@pl';
```

## Adding Relationships
_Should be definted during creation with REFERENCES_
```sql
create table person (
	first_name VARCHAR(50) NOT NULL,
	last_name VARCHAR(50) NOT NULL,
	email VARCHAR(150),
	gender VARCHAR(7) NOT NULL,
	date_of_birth DATE NOT NULL,
    car_id BIGINT REFERENCES car(id),
    UNIQUE(car_id)
);

create table car (
	id BIGSERIAL NOT NULL PRIMARY KEY,
	make VARCHAR(100) NOT NULL,
	model VARCHAR(100) NOT NULL,
	price NUMERIC(19,2) NOT NULL
);
```

## Inner Joins
_Combining Two Tables, results which exist in both_ 
_In the example: People Who has cars + Car Info_
```sql
SELECT * FROM person 
JOIN car ON person.car_id = car.id;
```

## Left Joins
_Combining Two Tables, results which exist in both and first table_ 
_In the example: All People + Car Info_
```sql
SELECT * FROM person 
LEFT JOIN car ON person.car_id = car.id;
```

## Extensions
_See All Extensions_
```sql
SELECT * FROM pg_available_extensions;
```

_Install_
```sql
CREATE EXTENSION IF NOT EXISTS "uuid-oosp";
```