# Azure-project4
# Joeri Vlieghe


Project 4 for azure data engineering - data pipelines for NYC payroll

### Project description
The City of New York would like to develop a Data Analytics platform on Azure Synapse Analytics to accomplish two primary objectives:

Analyze how the City's financial resources are allocated and how much of the City's budget is being devoted to overtime.
Make the data available to the interested public to show how the City’s budget is being spent on salary and overtime pay for all municipal employees.
You have been hired as a Data Engineer to create high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operation. The project team also includes the city’s quality assurance experts who will test the pipelines to find any errors and improve overall data quality.

The source data resides in Azure Data Lake and needs to be processed in a NYC data warehouse in Azure Synapse Analytics. The source datasets consist of CSV files with Employee master data and monthly payroll data entered by various City agencies.

<img src="screenshots/db-schema.jpeg" title="db schema" width="900">


# Tasks

## Task 1 : Create and Configure Resources

### Step 1: Prepare the Data Infrastructure

  Setup Data and Resources in Azure

#### 1. Create the data lake and upload data

I created a storage account and made sure to select "hierarchical namespace" to make it a gen2 data lake.
<img src="screenshots/storageaccount.png" title="db schema" width="900">

Next I uploaded the .csv files provided to the respective folders:

- dirpayrollfiles
- dirhistoryfiles
- dirstaging

<img src="screenshots/directories.png" title="db schema" width="900">

Upload these files from the project data to the dirpayrollfiles folder

- EmpMaster.csv
- AgencyMaster.csv
- TitleMaster.csv
- nycpayroll_2021.csv
  
<img src="screenshots/filesuploaded.png" title="db schema" width="900">

Upload this file (historical data) from the project data to the dirhistoryfiles folder

nycpayroll_2020.csv

#### 2. Create an Azure Data Factory Resource

I configured and created the data factory resource:

<img src="screenshots/createdf.png" title="db schema" width="900">

#### 3. Create a SQL Database to store the current year of the payroll data

In the Azure portal, create a SQL Database resource named db_nycpayroll

<img src="screenshots/createsql.png" title="db schema" width="900">

Add client IP address to the SQL DB firewall

Create a table called NYC_Payroll_Data in db_nycpayroll in the Azure Query Editor with this SQL Script:

```
CREATE TABLE [dbo].[NYC_Payroll_Data](
    [FiscalYear] [int] NULL,
    [PayrollNumber] [int] NULL,
    [AgencyID] [varchar](10) NULL,
    [AgencyName] [varchar](50) NULL,
    [EmployeeID] [varchar](10) NULL,
    [LastName] [varchar](20) NULL,
    [FirstName] [varchar](20) NULL,
    [AgencyStartDate] [date] NULL,
    [WorkLocationBorough] [varchar](50) NULL,
    [TitleCode] [varchar](10) NULL,
    [TitleDescription] [varchar](100) NULL,
    [LeaveStatusasofJune30] [varchar](50) NULL,
    [BaseSalary] [float] NULL,
    [PayBasis] [varchar](50) NULL,
    [RegularHours] [float] NULL,
    [RegularGrossPaid] [float] NULL,
    [OTHours] [float] NULL,
    [TotalOTPaid] [float] NULL,
    [TotalOtherPay] [float] NULL
)

GO
```
<img src="screenshots/createtablesql.png" title="db schema" width="900">

#### 4. Create a Synapse Analytics workspace

<img src="screenshots/createsynapse.png" title="db schema" width="900">

#### 5. Create master data tables and payroll transaction tables in Synapse Analytics workspace

I have created multiple sql scripts to create the necessary external file format, data sources and external tables.

<img src="screenshots/synapse_create_tables.png" title="db schema" width="900">

## Task 2 : Create Linked Services

#### 1. Create a Linked Service for Azure Data Lake

<img src="screenshots/linked_data_lake.png" title="db schema" width="900">

#### 2. Create a Linked Service to SQL Database that has the current (2021) data

<img src="screenshots/linked_sql.png" title="db schema" width="900">

#### 3. Create a Linked Service for Synapse Analytics

<img src="screenshots/linked_synapse.png" title="db schema" width="900">

## Task 3 : Create Datasets

#### 1. Create the datasets for the 2021 Payroll file on Azure Data Lake Gen2

<img src="screenshots/dataset_payroll.png" title="db schema" width="900">

#### 2. Repeat the same process to create datasets for the rest of the data files in the Data Lake

#### 3. Create the dataset for transaction data table that should contain current (2021) data in SQL DB

<img src="screenshots/dataset_sql.png" title="db schema" width="900">

#### 4. Create the datasets for destination (target) tables in Synapse Analytics

