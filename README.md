# Data Engineering Udacity Nanodegree
## Capstone Project

### Project Summary

The capstone project aims to answer questions about the immigration to U.S.. Questions about the immigrants profile, the most visa types issued by U.S. government, weather patterns and how they can influence the immigration's statistics are some of the answers we can get from the data provided for this project.

### Step 1: Scope the Project and Gather Data

#### Scope 
The project's goal is to use 3 datasets and manipulate them using Pyspark. By doing so, it will be created a data model proposed by Ralph Kimball called star schema. It will also be created 6 dimensional tables and 1 fact table.

#### Describe and Gather Data

For this project it was used 3 data sources:
- **I94 immigration data**: This data comes from the US National Tourism and Trade Office. A data dictionary is included in the workspace. [This](https://www.trade.gov/national-travel-and-tourism-office) is where the data comes from.
- **World Temperature Data**: The dataset comes from Kaggle. You can read more about it [here](https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data).
- **U.S. City Demographic Data**: This data comes from OpenSoft. You can read more about it [here](https://datahub.io/core/airport-codes#data).

### Step 2: Explore and Assess the Data
Please check it out the Jupyter Notebook.

### Step 3: Define the Data Model
#### 3.1 Conceptual Data Model
As previously mentioned, the chosen data model was the star schema, which was proposed by Ralph Kimball. That model was the chosen one because it allows great performance, which is desired since the dataset can increase, and it also allows users to write simple queries joining the fact and dimension tables in order to achieve the analytical dataset they need. The tables are as follow:

![Data Model1](assets/data_model2.drawio.svg)


#### 3.2 Mapping Out Data Pipelines
1. Create a Spark dataframe with SAS data.
2. Clean the data by removing duplicates, NA values, and renaming fields with more meaningful name.
3. Create dimension tables and fact tables.
4. Save the final tables on parquet format.

#### 4.3 Data dictionary 
Description of the data:

##### Dimension tables:
**country_table**
- country_code: integer that represents a country.
- country_name: name of a country.

**dim_visa**
- visa_id: An integer which represents a type of visa.
- visa_type: A properly type of visa issued by U.S.
- visa_category: A category the visa type belongs to.

**dim_date**
- date: date based on immigrant's arrival.
- year: year extracted from arrival's date.
- month: month extracted from arrival's date.
- week: week extracted from arrival's date.
- day: day extracted from arrival's date.

**dim_immigrant**
- id: unique value for an immigrant.
- age: immigrant's age.
- biryear: immigrant's year of birth.
- gender: immgrant's gender.

**dim_demographic**
- demo_id: unique value for demo info.
- port_code: port code where an immigrant arrived.
- state_code: state code where the port code is located.
- city: name of the city for a port code.
- median_age: median age for the corresponding city.
- male_population: number of male in a city.
- female_population: number of female in a city.
- total_population: total population for a city.
- number_of_veterans: total people considered veteran.
- foreign_born: number of people who was born abroad.
- avg_household_size: average size of a family living in a house.
- american_indian_and_alaska_native: number of indian and alaska native people for a city.
- asian: number of asian people for a city.
- black_or_african_american: number of black or african american people for a city.
- hispanic_or_latino: number of hispanic or latino people for a city.
- white: number of white people for a city.

**dim_temperature**
- id: unique value for temperature info in the table.
- city: city name where the average temperature was calculated.
- month: reference month when the average temperature was calculated.
- port_code: port code where an immigrant arrived.
- state_code: state code where the port code is located.
- average_temperature: monthly average temperature calculated.

##### Fact table
**fact_immigration**
- id: unique value for an immigration fact.
- arrival_date: the date an immigrant arrived in U.S.
- departure_date: the date an immigrant left in U.S.
- citizenship_code: code for the country an immigrant's citizenship.
- residence_code: code for the country an immigrant's residence.
- visa_id: unique value for a visa issued by U.S.
- demo_id: unique value for a demographic information.


#### 4.2 Data Quality Checks
Explain the data quality checks you'll perform to ensure the pipeline ran as expected. These could include:
 * Integrity constraints on the relational database (e.g., unique key, data type, etc.)
 * Unit tests for the scripts to ensure they are doing the right thing
 * Source/Count checks to ensure completeness
 
Run Quality Checks


#### Step 5: Complete Project Write Up
* Clearly state the rationale for the choice of tools and technologies for the project.
For this project the chosen technology was spark mainly because of the following:
    - Spark is easy-to-use and the learning curve fot those ones who already know pandas is not steep.
    - With spark it is possible to handle many different file formats.
    - Spark was built with the goal to work with big data, hence it is totally compatible with cloud technologies such as AWS.
    * Propose how often the data should be updated and why.
    - Since immigration data is updated monthly, so do our data.
* Write a description of how you would approach the problem differently under the following scenarios:
 * The data was increased by 100x.
     - Spark was built with the goal to handle big data. In this case, it would be interesting to work with cloud technology such as AWS. In case already working with cloud computing, we may consider increase the number of nodes. 
 * The data populates a dashboard that must be updated on a daily basis by 7am every day.
     - For this purpose, we could use Apache Airflow in order to schedule the pipeline.
 * The database needed to be accessed by 100+ people.
     - In this case, the recommended option would be use a cloud datawarehouse solution such as Amazon Redshift, but taking into consideration the costs involved.