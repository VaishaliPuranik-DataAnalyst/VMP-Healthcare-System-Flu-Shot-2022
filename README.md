# VMP-Healthcare-System-Flu-Shot-2022
## Introduction:
  Flu, also known as Influenza, is a viral infection of the respiratory system. It is contagious. Every flu season about 20-40 million people in the U.S. catch the flu. Most people with  the flu get better on their own. But sometimes, the flu and its complications can be deadly. Certain health conditions can put you at higher risk for severe illness from the flu. This includes life-threatening complications that require hospitalization. To help protect against seasonal flu, you can get an annual flu vaccination. 
##  Objective : 
 A Physician asked to build a flu vaccine dashboard for 2022 that does the following :
 
1). Total % of patients getting flu vaccines stratified by

        a). Age
        b). Race
        c). County (on a map)
        d). Overall
        
 2). Running Total of flu vaccines throughout 2022
 
 3). Total number of flu vaccines given in 2022
 
 4). A list of patients that shows whether or not they received the flu vaccines
 
 ####  Requirements :
Patients must have been "Active at our hospital"

## Data:
Dealing with healthcare data is tricky. A most prominent barrier is that ... because of HIPPA laws, it is largely unavailable to the public.

In this project, I used the data from "Real World Fake Data" (RWFD). The data is synthetically generated using the project called Synthea. It is fake data that is generated to look like it is real. While it can sometimes be a bit off from the real expectations, it is largely a good depiction of the real counterpart. 

The entire dataset contains 15 tables and 6+ millions of rows. In this particular project, I mainly used three tables:

1. immunizations: This table contains information about the different vaccines patients received.
2. patients: This table contains the demographic information of the patients.
3. encounters: This table contains information about the unique patient interaction with the healthcare system.

##  Creating the Database and Tables
I used the available database "postgres" and schema "public". 

### Creating Tables in the database:
 I created the 3 tables, patients, encounters, and immunizations using the following code:
 ``` SQL
--  Creating table encounters

CREATE TABLE IF NOT EXISTS encounters 
(
	id VARCHAR(100),
	start TIMESTAMP,
	stop TIMESTAMP,
	patient VARCHAR(100),
	organization VARCHAR(100),
	provider VARCHAR(100),
	payer VARCHAR(100),
	encounterclass VARCHAR(100),
	code VARCHAR(100),
	description VARCHAR(100),
	base_encounter_class FLOAT,
	total_claim_cost FLOAT,
	payer_coverage FLOAT,
	reasoncode VARCHAR(100)
	--reasondescription VARCHAR(100)
);
-- Creating table immunizations

CREATE TABLE IF NOT EXISTS immunizations
(
	date TIMESTAMP,
	patient varchar(100),
	encounter varchar(100),
	code int,
	description varchar(500)
	--BASE_COST float
);

-- Creating Table patients

CREATE TABLE IF NOT EXISTS patients
(
	id VARCHAR(100),
	birthdate date,
	deathdate date,
	ssn VARCHAR(100),
	drivers VARCHAR(100),
	passport VARCHAR(100),
	prefix VARCHAR(100),
	first VARCHAR(100),
	last VARCHAR(100),
	suffix VARCHAR(100),
	maiden VARCHAR(100),
	marital VARCHAR(100),
	race VARCHAR(100),
	ethnicity VARCHAR(100),
	gender VARCHAR(100),
	birthplace VARCHAR(100),
	address VARCHAR(100),
	city VARCHAR(100),
	state VARCHAR(100),
	county VARCHAR(100),
	fips INT, 
	zip INT,
	lat float,
	lon float,
	healthcare_expenses float,
	healthcare_coverage float,
	income int,
	mrn int
);
```
## Importing data into  :

	
Import the data into the tables:
After creating tables using SQL query, 
	a. Right-click on tables under public schema and go to tables
	b. Right-click on one of the tables, click on import/export data
In Import/Export data-table conditions,
	c. In IMPORT, click on the filename and choose the file from where you save the data file, Check the format as a text file or JSON file etc. Here it's .txt file
	d. In Options, check for OID off and Header On, Deliminiter (separated by), or ., Quote as ", Escape as ', then NULL String as \N
	d. If everything looks good, click ok in General, and all the data gets imported or loaded into the table.
	
Repeat these steps for each table to load data.

	1. Right-click the table to fill it with imported data.
	2. Point to Tools and select Import Data.
	3. Select the file to import, specify its format, and click Next.
	4. Choose the existing tables as a destination for import.
	5. Click Import.

 ## Verifying data:
 ``` SQL
	select * from "postgres"."public".patients
```
! [Screenshot of Patient's data] ()
 

 ## Reference :
 https://my.clevelandclinic.org/health/diseases/4335-influenza-flu
 
 https://www.mayoclinic.org/diseases-conditions/flu/symptoms-causes/syc-20351719





    
