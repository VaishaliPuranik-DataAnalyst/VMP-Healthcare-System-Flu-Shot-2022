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

I used the "Real World Fake Data" (RWFD) data in this project. The data is synthetically generated using the project called Synthea. It is fake data that is generated to look like it is real. While it can sometimes be a bit off from the real expectations, it is largely a good depiction of the real counterpart. This fictional healthcare system takes place in the state of Massachusetts.

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
1). Data from **patients** table:
 ``` SQL
	select * from "postgres"."public".patients
```
![Screenshot 2025-02-11 223801](https://github.com/user-attachments/assets/82942834-f80b-4e9f-b360-f8778523af85)


2). Data from **encounters** table:

``` SQL
	select * from "postgres"."public".encounters
```
![encounters table data](https://github.com/user-attachments/assets/9ea53a91-5e4d-46b7-affc-365c97e627ea)


3). Data from immunizations table:

``` SQL
	select * from "postgres"."public".immunizations
```
![immunizations table data](https://github.com/user-attachments/assets/5be3a96e-e0e9-4504-9a4a-89c56bd8f044)

## Data Cleaning :
In this project, the data is synthetically generated, It is mostly clean. I still verified the date format in columns such as date in the immunizations table, start and stop columns in the encounters table, and birthdate and deathdate columns in the patients table.

## Data extracting :
I get the patient's demographic information like age, race, county, first and last name, id from patients table. I want to use joins using id as a primary key in patients table and patient as a foreign key in immunizations table, to connect the other tables such as immunizations table to get information about patients who did or did not receive flu shots. Since it's one to many joins, there is a chance that I can get repetitive data/ rows and mess up the calculation of %s of patients getting flu shots. Instead, I used CTES for active patients in 2020 -2022, and flu shots given in 2022. Then join these CTEs or use as a subquery in the main query.

#### 1). CTE for Active Patients in 2020-2022
Active patients mean patients who have had any encounter between 2020 and 2022 and are alive and >= 6 months. Recommended practice is for patients older than 6 months to get the flu shot.

In this CTE, I used join to connect encounters table to patients table to get the patients who are alive and to calculate the age in months for patients who are older than 6 months.
``` SQL
 with active_patients as 
 (
 select distinct patient
 from "postgres"."public".encounters as enc
 join "postgres"."public".patients as pat
 on enc.patient = pat.id
 where enc.start between '2020-01-01 00:00' and '2022-12-31 23:59'
 and pat.deathdate is null
 and extract (EPOCH from age('2022-12-31 23:59', pat.birthdate)) >= 6
 /* Recommended practice is Patients older than 6 months get the flu shot.*/
 ),
```
#### 2). CTE for Flu Shots Given in 2022
Some people get multiple flu shots in a year. This CTE gives the patient's id and the earliest date when the flu shot was given.

``` SQL
 flu_shot_2022 as
 (
 select patient,
	min(date) as earliest_flu_shot_2022
 from "postgres"."public".immunizations
 where code = 5302
 and date between '2022-01-01 00:00' and '2022-12-31 23:59'
 group by patient
 ),
```
#### Main Query
In the main query, I used patients table to get the patient's demographic information (age, race, county, id, first, last) and used **left join** to connect to Flu_Shot_Given_2022 CTE. Then I also used the active_patients CTE as a subquery in **WHERE** clause to get active patients. I also used **CASE** statement to know if the patient received the flu shot or not.
```SQL
select  pat.birthdate,
	pat.race,
	pat.county,
	pat.id,
	pat.first,
	pat.last,
	flu.patient,
	flu.earliest_flu_shot_2022,
	extract(year from age('12-31-2022 23:59', pat.birthdate)) as age, 
	case when flu.patient is not null then 1 
	else 0
	end as flu_shot_2022		 
 from "postgres"."public".patients as pat
 left join flu_shot_2022 as flu
 on pat.id = flu.patient
where 1=1
and pat.id in (select patient from active_patients)
/* Here we use active_patients CTE as a subquery. */
```

I saved this extracted data as Flu_Shot_data.csv and then used in **Tableau** for visualization.

## Making a Flu_Shot 2022 Dashboard in Tableau
https://public.tableau.com/app/profile/vaish.p/viz/VMPHealthacreSystemFluShots2022/Dashboard1








 ## Reference :
 https://my.clevelandclinic.org/health/diseases/4335-influenza-flu
 
 https://www.mayoclinic.org/diseases-conditions/flu/symptoms-causes/syc-20351719





    
