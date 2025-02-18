# Feature Engineering

1. Import packages
2. Load data
3. Feature engineering
4. Enhance a Dataset

## 1. Import packages

### I. Importing Library
```
import pandas as pd
```
- This imports the Pandas library and gives it the alias `pd`.
- Pandas is a powerful data analysis and manipulation library in Python.

## 2. Load data

### I. Reading a CSV File
```
df = pd.read_csv('./clean_data_after_eda.csv')
```
- `pd.read_csv()`: This function is used to read a CSV (Comma-Separated Values) file and store it in a DataFrame (a tabular data structure similar to an Excel sheet).
- `'./clean_data_after_eda.csv'` is the file path. The `./` means the file is in the current directory.
- `df` is the DataFrame that will store the data.

### II. Converting String Columns to Datetime Format
```
df["date_activ"] = pd.to_datetime(df["date_activ"], format='%Y-%m-%d')
df["date_end"] = pd.to_datetime(df["date_end"], format='%Y-%m-%d')
df["date_modif_prod"] = pd.to_datetime(df["date_modif_prod"], format='%Y-%m-%d')
df["date_renewal"] = pd.to_datetime(df["date_renewal"], format='%Y-%m-%d')
```
- The `pd.to_datetime()` function converts a column from a string (text format) to a datetime format.
- The `format='%Y-%m-%d'` specifies that the date format is Year-Month-Day (e.g., `2023-05-15`).
- These transformations are applied to four columns:
  - `date_activ` - Likely represents the activation date of something.
  - `date_end` - Likely represents the end date of something.
  - `date_modif_prod` - Likely represents the modification date of a product.
  - `date_renewal` - Likely represents the renewal date of something.

### III. Displaying the First 3 Rows
```
df.head(3)
```
- The `.head(n)` function returns the first `n` rows of the DataFrame.
- `df.head(3)` displays the first 3 rows of `df`.
- This is useful to preview the dataset and check if the CSV was read correctly.

#### Output:
![Screenshot 2025-02-18 at 3 10 51 pm](https://github.com/user-attachments/assets/b2d87fd3-6549-4e5a-a72f-93ba9e15ebb1)
![Screenshot 2025-02-18 at 3 11 07 pm](https://github.com/user-attachments/assets/fa7f51cb-63e2-4ec5-9ca0-05517cdd130d)
![Screenshot 2025-02-18 at 3 11 23 pm](https://github.com/user-attachments/assets/0c60fd2a-cf80-473f-bd96-9c2462337154)

## 3. Feature engineering

### I. Reading a CSV File
```
price_df = pd.read_csv('price_data.csv')
```
- `pd.read_csv('price_data.csv')` loads data from the CSV file named `'price_data.csv'` into a DataFrame (`price_df`).
- A CSV (Comma-Separated Values) file stores tabular data where values are separated by commas.
- `price_df` now holds this tabular data in memory.

### II. Converting the price_date Column to Date Format
```
price_df["price_date"] = pd.to_datetime(price_df["price_date"], format='%Y-%m-%d')
```
- `pd.to_datetime()` converts the `price_date` column from a string format (text) to a datetime object.
- The format specified is:
  - `%Y-%m-%d` → Year (`%Y`), Month (`%m`), and Day (`%d`) (e.g., `2024-01-01`).

### III. Displaying the First Few Rows
```
price_df.head()
```
- `.head()` displays the first 5 rows of the DataFrame.
- It helps verify that:
  - The CSV file loaded correctly.
  - The `price_date` column is now in datetime format.
- <Output>
- The price_date column is now a datetime object, not a string.

#### Output:
![Screenshot 2025-02-18 at 3 13 51 pm](https://github.com/user-attachments/assets/3d4ec6f7-0134-4c7f-8edb-0b29c2495977)

### IV. Grouping Off-Peak Prices by Companies and Month
```
monthly_price_by_id = price_df.groupby(['id', 'price_date']).agg({'price_off_peak_var': 'mean', 'price_off_peak_fix': 'mean'}).reset_index()
```
- `groupby(['id', 'price_date'])`:
  - Groups the `price_df` DataFrame by `id` (company identifier) and `price_date` (month).
  - This means we will calculate values for each company (`id`) per month (`price_date`).
- `.agg({'price_off_peak_var': 'mean', 'price_off_peak_fix': 'mean'})`:
  - `price_off_peak_var` → Takes the mean of off-peak variable prices.
  - `price_off_peak_fix` → Takes the mean of off-peak fixed prices.
  - If there are multiple records for a company in a month, we take the average.
- `.reset_index()`:
  - Converts the grouped data back into a DataFrame instead of a grouped object.

### V. Extracting January and December Prices
```
jan_prices = monthly_price_by_id.groupby('id').first().reset_index()
dec_prices = monthly_price_by_id.groupby('id').last().reset_index()
```
- `.groupby('id').first()`:
  - Groups data by `id` (company).
  - Picks the first row for each company (January price, assuming data is sorted).
- `.groupby('id').last()`:
  - Groups data by `id`.
  - Picks the last row for each company (December price).
- `.reset_index()`:
  - Converts the grouped data back into a DataFrame.

### VI. Merging January and December Prices
```
diff = pd.merge(dec_prices.rename(columns={'price_off_peak_var': 'dec_1', 'price_off_peak_fix': 'dec_2'}), jan_prices.drop(columns='price_date'), on='id')
```
- `dec_prices.rename(...)`:
  - Renames columns:
    - `price_off_peak_var → dec_1`
    - `price_off_peak_fix → dec_2`
  - Helps distinguish December data from January data.
- `jan_prices.drop(columns='price_date')`:
  - Drops the `price_date` column from January prices (since we only need price values).
- `pd.merge(..., on='id')`:
  - Merges `dec_prices` (renamed columns) with `jan_prices` on `id`.
  - This aligns January and December prices for each company.

### VII. Calculating the Difference Between December and January Prices
```
diff['offpeak_diff_dec_january_energy'] = diff['dec_1'] - diff['price_off_peak_var']
diff['offpeak_diff_dec_january_power'] = diff['dec_2'] - diff['price_off_peak_fix']
```
- Calculating Difference in Energy Prices (`price_off_peak_var`):
  - `offpeak_diff_dec_january_energy = dec_1 - price_off_peak_var`
  - (December's off-peak variable price - January's off-peak variable price).
- Calculating Difference in Power Prices (`price_off_peak_fix`):
  - `offpeak_diff_dec_january_power = dec_2 - price_off_peak_fix`
  - (December's off-peak fixed price - January's off-peak fixed price).

### VIII. Keeping Only Relevant Columns
```
diff = diff[['id', 'offpeak_diff_dec_january_energy','offpeak_diff_dec_january_power']]
```
- This removes unnecessary columns (`price_date`, `dec_1`, `dec_2`).
- Keeps only:
  - `id` (company)
  - `offpeak_diff_dec_january_energy` (difference in off-peak energy prices)
  - `offpeak_diff_dec_january_power` (difference in off-peak power prices)

### IX. Displaying the First Few Rows
```
diff.head()
```
- Displays the first few rows of `diff` to verify the results.

#### Output:
![Screenshot 2025-02-18 at 3 14 38 pm](https://github.com/user-attachments/assets/31bbe99e-8a43-4475-b861-338268a219af)

## 4. Enhances a Dataset

### I. Import Libraries
```
import pandas as pd
```
- pandas (`pd`) is a powerful Python library for data manipulation and analysis.

### II. Load the Dataset
```
df = pd.read_csv("clean_data_after_eda.csv")
```
- Reads a CSV file (`clean_data_after_eda.csv`) and loads it into a Pandas DataFrame (`df`).
- `df` now holds a tabular dataset.

### III. Convert Date Columns to Datetime Format
```
date_cols = ["date_activ", "date_end", "date_modif_prod", "date_renewal"]
for col in date_cols:
    df[col] = pd.to_datetime(df[col], format="%Y-%m-%d")
```
- Converts date columns from string format (e.g., `"2023-01-01"`) to Pandas datetime format.
  - Enables date operations like extracting year, month, and calculating differences.
- The `for` loop ensures that all specified columns are converted.

### IV. Feature 1: Extract Year, Month, and Day from Date Columns
```
for col in date_cols:
    df[f"{col}_year"] = df[col].dt.year
    df[f"{col}_month"] = df[col].dt.month
    df[f"{col}_day"] = df[col].dt.day
```
- Extracts year, month, and day for each date column and creates new columns.
- This helps in trend analysis (e.g., identifying seasonal patterns).

### V. Feature 2: Calculate Contract Duration (days between activation and end date)
```
df["contract_duration"] = (df["date_end"] - df["date_activ"]).dt.days
```
- Calculates the contract duration in days by subtracting `date_activ` from `date_end`.
- `.dt.days` extracts the result as an integer.

### VI. Feature 3: Calculate Consumption Ratio (last month vs. 12 months)
```
df["consumption_ratio"] = df["cons_last_month"] / (df["cons_12m"] + 1)  # Avoid division by zero
```
- Computes the ratio of last month's consumption to the last 12 months.
- Why `+1` in denominator?
  - To prevent division by zero in case `cons_12m = 0`.

### VII. Feature 4: Calculate Price Volatility (standard deviation of 6-month price variations)
```
price_cols = ["var_6m_price_off_peak", "var_6m_price_peak", "var_6m_price_mid_peak"]
df["price_volatility"] = df[price_cols].std(axis=1)
```
- Computes standard deviation of 6-month price variations.
- `std(axis=1)` calculates row-wise standard deviation across selected columns.
- This measures price fluctuation, useful for risk analysis.

### VIII. Feature 5: Encode 'has_gas' as Binary
```
df["has_gas_binary"] = df["has_gas"].map({"t": 1, "f": 0})
```
- Converts categorical column (`"t"` or `"f"`) to binary (1 or 0).
  - Many ML models require numerical inputs.

### IX. Save the Enhanced Dataset
```
df.to_csv("enhanced_features.csv", index=False)
```
- Saves the enhanced DataFrame to a new CSV file (`enhanced_features.csv`).
- `index=False` prevents Pandas from saving the default row index.

### X. Load the Enhanced Dataset
```
enhance_df = pd.read_csv("enhanced_features.csv")
```
- Reads the `enhanced_features.csv` file into a Pandas DataFrame called `enhance_df`.
- This dataset is the processed/enhanced version of `clean_data_after_eda.csv`, with new features like:
  - Extracted year, month, day from date columns.
  - Contract duration calculation.
  - Consumption ratio.
  - Price volatility.
  - Binary encoding for `has_gas`.

### XI. Display First Few Rows
```
enhance_df.head()
```
- Shows the first 5 rows of `enhance_df`.
- Helps quickly inspect the dataset structure.

#### Output:
![Screenshot 2025-02-18 at 3 23 19 pm](https://github.com/user-attachments/assets/07c5179c-318c-4f17-bfc9-2c841c3ba873)
![Screenshot 2025-02-18 at 3 23 37 pm](https://github.com/user-attachments/assets/c4f33a83-4190-479b-93e6-b8e397c600f9)

### XII. Display DataFrame Information
```
enhance_df.info()
```
- Provides summary information about `enhance_df`, including:
  - Column names
  - Data types (e.g., `int64`, `float64`, `object`, `datetime64`)
  - Non-null values per column

#### Output:
![Screenshot 2025-02-18 at 3 24 17 pm](https://github.com/user-attachments/assets/27b15613-98af-4b56-9970-90699be0ff74)
![Screenshot 2025-02-18 at 3 24 32 pm](https://github.com/user-attachments/assets/bd04fd7c-1622-4e03-a3b3-d8618fecf7ed)

### XIII. Load the Original Dataset
```
df = pd.read_csv("clean_data_after_eda.csv")
```
- Reads the original dataset into a Pandas DataFrame called `df`.
- This dataset does not contain enhancements like extracted date parts, contract duration, etc.

### IX. Display First Few Rows of Original Data
```
df.head()
```
- Displays the first 5 rows of `df`.
- This allows comparison with `enhance_df`.

#### Output:
![Screenshot 2025-02-18 at 3 25 27 pm](https://github.com/user-attachments/assets/bb4c841a-a9d9-4374-9eb2-85c492acf681)
![Screenshot 2025-02-18 at 3 25 48 pm](https://github.com/user-attachments/assets/919b0f1c-01d1-4700-8ae4-60269de584c0)

### X. Display DataFrame Information for Original Data
```
df.info()
```
- Shows a summary of `df`, similar to `enhance_df.info()`.

#### Output:
![Screenshot 2025-02-18 at 3 27 02 pm](https://github.com/user-attachments/assets/f31dd629-1c9f-4a91-afc4-b457864c00a1)

---
