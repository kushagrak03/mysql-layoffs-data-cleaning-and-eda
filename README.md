# 📊 Layoffs Data Cleaning & Exploratory Data Analysis (MySQL)

An end-to-end SQL project on a global company layoffs dataset. The first half cleans messy raw data into an analysis-ready table, and the second half explores it to find layoff trends across companies, industries, countries, funding stages and time.

## 📌 Project Overview

| Phase | Goal | File |
|-------|------|------|
| 1. Data Cleaning | Turn raw, messy data into a reliable table | `data_cleaning.sql` |
| 2. Exploratory Data Analysis | Find trends and patterns in the cleaned data | `eda.sql` |

## 📂 Repository Structure

```
├── layoffs.csv           # Raw dataset
├── data_cleaning.sql     # Phase 1: cleaning queries
├── eda.sql               # Phase 2: exploratory analysis queries
└── README.md
```

## 🗂️ Dataset

Columns: `company`, `location`, `industry`, `total_laid_off`, `percentage_laid_off`, `date`, `stage`, `country`, `funds_raised_millions`.

## 🛠️ Tools & Skills Used

- **MySQL** (MySQL Workbench)
- **Window functions:** `ROW_NUMBER()`, `DENSE_RANK()`, rolling `SUM() OVER()`
- **CTEs** (including multi-step CTEs)
- **Self-joins** to fill missing values
- **Aggregations:** `GROUP BY`, `SUM`, `MAX`, `MIN`
- **String and date functions:** `TRIM`, `SUBSTRING`, `STR_TO_DATE`, `YEAR`
- **DDL / DML:** `CREATE TABLE ... LIKE`, `ALTER TABLE`, `UPDATE`, `DELETE`

---

## 🧹 Phase 1: Data Cleaning

0. **Staging table:** created `layoffs_staging` as a copy so the raw data is never modified.
1. **Removed duplicates:** used `ROW_NUMBER()` partitioned across all columns to flag duplicates. Since MySQL doesn't allow deleting from a CTE, I created `layoffs_staging2` with a `row_num` column and deleted duplicates there.
2. **Standardized data:**
   - Trimmed whitespace in `company`
   - Merged industry variants (`Crypto`, `Crypto Currency`, `CryptoCurrency`) into `Crypto`
   - Removed the trailing period from `United States.`
   - Converted `date` from text to a real `DATE` using `STR_TO_DATE` and `ALTER TABLE`
3. **Handled NULL and blank values:** converted blank industries to `NULL`, then used a **self-join** to fill them from other rows of the same company.
4. **Removed unnecessary data:** deleted rows with no layoff information (both `total_laid_off` and `percentage_laid_off` NULL) and dropped the helper `row_num` column.

---

## 🔍 Phase 2: Exploratory Data Analysis

Questions explored on the cleaned table:

- What are the maximum layoff numbers and percentages?
- Which companies shut down completely (`percentage_laid_off = 1`), and how much funding had they raised?
- Which **companies**, **countries**, **industries** and **funding stages** had the most layoffs?
- What is the date range of the data, and how do layoffs change **year by year**?
- What is the **month-by-month** trend, and the **rolling total** over time?
- Who are the **top companies by layoffs in each year**? (`DENSE_RANK()` partitioned by year)

### Key Findings

> Replace these with your real results from running `eda.sql`.

- **Date range covered:** [start date] to [end date]
- **Company with the most layoffs:** [company] ([number])
- **Country with the most layoffs:** [country] ([number or %])
- **Year with the most layoffs:** [year] ([number])
- **Stage hit hardest:** [stage]
- **Total layoffs across the dataset (rolling total, final month):** [number]

---

## ▶️ How to Run

1. Create a schema and import `layoffs.csv` into a table named `layoffs` (MySQL Workbench → Table Data Import Wizard).
2. Run `data_cleaning.sql` top to bottom. This creates the `layoffs_staging2` table.
3. Run `eda.sql` on the cleaned table.

## 🔮 Possible Next Steps

Visualize the results in Power BI or Excel (layoffs by year, top companies, industry trends).

## 👤 Author

**Kushagra Kumar**
Electronics & Telecommunications Engineering student | Data & Marketing enthusiast
🔗 [LinkedIn](https://www.linkedin.com/in/kushagra-kumar-a3971224b) · 📧 kushagrak00@gmail.com
