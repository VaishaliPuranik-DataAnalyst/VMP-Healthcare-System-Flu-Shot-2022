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
 ``` sql
CREATE TABLE encounters 
(
	Id VARCHAR(100)
	,START TIMESTAMP
	,STOP TIMESTAMP
	,PATIENT VARCHAR(100)
		,ORGANIZATION VARCHAR(100)
		,PROVIDER VARCHAR(100)
		,PAYER VARCHAR(100)
		,ENCOUNTERCLASS VARCHAR(100)
		,CODE VARCHAR(100)
		,DESCRIPTION VARCHAR(100)
		,BASE_ENCOUNTER_COST FLOAT
		,TOTAL_CLAIM_COST FLOAT
		,PAYER_COVERAGE FLOAT
		,REASONCODE VARCHAR(100)
		--,REASONDESCRIPTION VARCHAR(100)
	);
```

 
    
