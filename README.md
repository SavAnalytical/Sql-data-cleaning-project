## SQL Data Cleaning Project: Layoffs Dataset

### Overview

This project demonstrates a complete data cleaning workflow using **MySQL**. The objective was to transform a raw layoffs dataset into a clean, consistent, and analysis-ready dataset by applying industry-standard SQL data cleaning techniques.

The cleaning process includes creating a staging table, identifying and removing duplicate records, standardizing inconsistent values, converting data types, handling missing values, and preparing the dataset for exploratory data analysis (EDA).

---

### Table of Contents

- [Dataset](#dataset)

- [Project Objectives](#project-objectives)

- [SQL Skills Demonstrated](#sql-skills-demonstrated)

- [Data Cleaning Workflow](#data-cleaning-workflow)

- [Project Steps](#project-steps)

- [Convert Date Format](#convert-date-format)


## Dataset

The project uses the  **Layoffs** dataset containing company layoff records from different industries and countries.

The dataset includes information such as:

* Company
* Location
* Industry
* Total Employees Laid Off
* Percentage Laid Off
* Date
* Company Stage
* Country
* Funds Raised (Millions)

---

## Project Objectives

The primary objectives of this project were to:

* Preserve the original dataset by creating a staging table.
* Detect and remove duplicate records.
* Standardize inconsistent text values.
* Correct date formats and convert data types.
* Handle null and blank values.
* Remove unnecessary columns.
* Produce a clean dataset suitable for analysis.

---

## SQL Skills Demonstrated

Throughout this project, the following SQL concepts were applied:

* CREATE TABLE
* INSERT INTO
* Window Functions (`ROW_NUMBER()`)
* Common Table Expressions (CTEs)
* DELETE
* UPDATE
* ALTER TABLE
* Self JOIN
* String Functions (`TRIM`)
* Date Functions (`STR_TO_DATE`)
* Data Type Conversion
* NULL Handling
* Data Standardization

---

## Data Cleaning Workflow

The project follows a structured ETL-style workflow.

```
Raw Dataset
      │
      ▼
Create Staging Table
      │
      ▼
Copy Original Data
      │
      ▼
Identify Duplicate Records
      │
      ▼
Remove Duplicate Records
      │
      ▼
Standardize Text Values
      │
      ▼
Convert Date Format
      │
      ▼
Handle Missing Values
      │
      ▼
Remove Unnecessary Columns
      │
      ▼
Final Clean Dataset
```

---

## Project Steps

## 1. Create a Staging Table

To protect the integrity of the original dataset, a staging table was created using the structure of the original table.

This ensured that all cleaning operations were performed on a working copy rather than the raw data.

---

## 2. Copy Data into the Staging Table

All records from the original table were copied into the staging table.

This approach preserves the raw dataset for future reference and reproducibility.

---

## 3. Identify Duplicate Records

Duplicate records were detected using the `ROW_NUMBER()` window function.

Each record was partitioned using multiple columns to determine whether duplicate rows existed.

Records with a row number greater than one were classified as duplicates.

```{sql connection=}
select*,
ROW_NUMBER() over(
partition by company, location, industry, total_laid_off, percentage_laid_off, 'date', stage, country, funds_raised_millions) as row_num
from layoffs_staging;
```

---

## 4. Remove Duplicate Records

A second staging table containing the generated row numbers was created.

Duplicate rows were removed using the generated identifiers while preserving one unique record for each duplicated group.

---

## 5. Standardize Data

Several inconsistencies were corrected to improve data quality.

### Company Names

Leading and trailing white spaces were removed using the `TRIM()` function.

Example:

```
" Airbnb"
```

became

```
"Airbnb"
```

### Industry

Different variations of the Crypto industry, such as:

* Crypto
* Crypto Currency
* Cryptocurrency

were standardized into a single value:

```
Crypto
```

### Country

Country names were standardized by removing trailing punctuation.

Example:

```
United States.
```

became

```
United States
```

---

## 6. Convert Date Format

The original dataset stored dates as text.

Using `STR_TO_DATE()`, the values were converted into the proper SQL DATE format before modifying the column data type.

```
select`date`,
str_to_date(`date`, '%m/%d/%Y')
from layoffs_staging2;
```

```
update layoffs_staging2
set `date` = str_to_date(`date`, '%m/%d/%Y');
```


This enables chronological filtering, sorting, and time-based analysis.

---

## 7. Handle Missing Values

Blank strings were converted into NULL values for consistency.

Missing industries were populated by performing a self-join on the company name.

This allowed records belonging to the same company to inherit existing industry values.

Rows containing no useful layoff information (both Total Laid Off and Percentage Laid Off were NULL) were removed from the dataset.

---

## 8. Remove Unnecessary Columns

After duplicate removal, the helper column (`row_num`) was no longer required and was dropped from the final dataset.

---



# Author

**Mbah Francis**
*

































