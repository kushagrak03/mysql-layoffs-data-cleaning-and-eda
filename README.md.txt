# 🧹 Layoffs Data Cleaning Project (MySQL)

A complete data cleaning workflow in **MySQL** on a global company layoffs dataset. The raw data contains duplicates, inconsistent text, text-formatted dates, and missing values. This project turns it into a clean, analysis-ready table.

## 📌 Project Overview

Raw data is rarely ready for analysis. This project walks through a structured, repeatable cleaning process using only SQL:

1. Remove duplicates
2. Standardize the data
3. Handle NULL and blank values
4. Remove unnecessary rows and columns

## 📂 Repository Structure

```
├── layoffs.csv           # Raw dataset
├── data_cleaning.sql     # All cleaning queries, in order
└── README.md
```

## 🗂️ Dataset

Columns: `company`, `location`, `industry`, `total_laid_off`, `percentage_laid_off`, `date`, `stage`, `country`, `funds_raised_millions`.

## 🛠️ Tools & Skills Used

- **MySQL** (MySQL Workbench)
- **Window functions:** `ROW_NUMBER() OVER (PARTITION BY ...)`
- **CTEs** (Common Table Expressions)
- **Self-joins** to fill missing values
- **String functions:** `TRIM`, `TRIM(TRAILING ...)`, `LIKE`
- **Date functions:** `STR_TO_DATE`
- **DDL / DML:** `CREATE TABLE ... LIKE`, `ALTER TABLE`, `UPDATE`, `DELETE`

## 🔄 Cleaning Process

### 0. Staging table
Created `layoffs_staging` as a copy of the raw table so the original data is never modified.

### 1. Removing duplicates
- Used `ROW_NUMBER()` partitioned across all columns to flag duplicate rows (`row_num > 1`).
- Since MySQL doesn't allow deleting directly from a CTE, created `layoffs_staging2` with an extra `row_num` column, inserted the numbered data, and deleted the duplicates there.

### 2. Standardizing data
- **Whitespace:** trimmed leading/trailing spaces in `company`.
- **Industry:** merged variants such as `Crypto`, `Crypto Currency` and `CryptoCurrency` into one `Crypto` label.
- **Country:** removed the trailing period in `United States.`.
- **Dates:** converted `date` from text to a proper `DATE` using `STR_TO_DATE`, then changed the column type with `ALTER TABLE`.

### 3. NULL and blank values
- Converted blank industry values to `NULL`.
- Used a **self-join** to fill missing `industry` values from other rows of the same company (e.g. Airbnb).

### 4. Removing unnecessary data
- Deleted rows where both `total_laid_off` and `percentage_laid_off` are `NULL`, since they carry no usable layoff information.
- Dropped the helper `row_num` column.

## ✅ Result

A clean `layoffs_staging2` table with:
- No duplicate records
- Consistent company, industry and country values
- A true `DATE` column
- Filled-in industries where recoverable
- No rows without layoff information

## ▶️ How to Run

1. Create a schema and import `layoffs.csv` as a table named `layoffs` (MySQL Workbench → Table Data Import Wizard).
2. Open `data_cleaning.sql` and run it step by step, top to bottom.

## 🔮 Next Steps

Exploratory data analysis on the cleaned table: layoffs by industry, country and year, top companies by layoffs, and rolling totals over time.

## 👤 Author

**Kushagra Kumar**
Electronics & Telecommunications Engineering student | Data & Marketing enthusiast
🔗 [LinkedIn](your-linkedin-link) · 📧 your-email@example.com