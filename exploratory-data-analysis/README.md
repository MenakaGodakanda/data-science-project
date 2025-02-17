# Exploratory Data Analysis

1. Import packages
2. Loading data with Pandas
3. Descriptive statistics of data
4. Data visualization

## 1. Import packages
Setting up a Jupyter Notebook environment for data visualization:
- Suppressing unnecessary FutureWarnings.
- Importing key libraries for data handling (`pandas`) and visualization (`matplotlib` and `seaborn`).
- Ensuring that plots are displayed inline in Jupyter.
- Setting a consistent plot style using `seaborn`.

### I. Suppressing FutureWarnings
```
import warnings
warnings.filterwarnings("ignore", category=FutureWarning)
```
- `warnings`: This module is part of Python’s standard library and is used to handle warnings (e.g., deprecated features, runtime warnings).
- `filterwarnings("ignore", category=FutureWarning)`: This line tells Python to ignore all warnings that belong to the `FutureWarning` category.
    - FutureWarning warnings are often raised when a feature in a library (such as `pandas` or `numpy`) is planned to change or be deprecated in future releases.
    - Ignoring them prevents clutter in the output but should be used with caution, as you may miss important updates.

### II. Importing Libraries
```
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
```
- `matplotlib.pyplot` (`plt`):
    - `matplotlib` is a fundamental plotting library in Python.
    - `pyplot` is a module in `matplotlib` that provides functions for creating plots.
- `seaborn` (`sns`):
    - `seaborn` is a statistical data visualization library built on top of `matplotlib`.
    - It provides more aesthetically pleasing and easy-to-use charting functions.
- `pandas` (`pd`):
    - pandas is a powerful data analysis and manipulation library.
    - It provides data structures like DataFrame and Series for handling structured data.

### III. Enabling Inline Plot Display
```
%matplotlib inline
```
- This is a magic command used in Jupyter Notebooks.
- It ensures that plots generated using `matplotlib` are displayed directly within the notebook output.
- Without this, plots might open in a separate window.

### IV. Setting Seaborn Plot Style
```
sns.set(color_codes=True)
```
- This sets the default style for `seaborn` plots.
- `color_codes=True`:
  - This allows using color abbreviations (`b` for blue, `r` for red, etc.) in `seaborn` plots.
  - Ensures that `seaborn` applies a consistent color scheme across plots.


## 2. Loading data with Pandas
Common initial data exploration workflow in data science and analytics:
- Loads CSV files (`client_data.csv` and `price_data.csv`) into pandas DataFrames.
- Displays the first 3 rows to check data (`head(3)`).
- Shows structure & data types (`info()`).
- Provides statistical insights on numeric columns (`describe()`).

### I. Reading CSV Files into DataFrames
```
client_df = pd.read_csv('./client_data.csv')
price_df = pd.read_csv('./price_data.csv')
```
- `pd.read_csv(filepath)`:
    - This function reads a CSV (Comma-Separated Values) file and loads it into a pandas DataFrame.
    - A DataFrame is a tabular data structure with rows and columns (like an Excel spreadsheet).
    - The `./` before the file name indicates that the file is in the current directory.
- Variables:
    - `client_df`: Stores the data from `client_data.csv`.
    - `price_df`: Stores the data from `price_data.csv`.

### II. Displaying the First Few Rows
#### Client Data:
```
client_df.head(3)
```
- `head(n)`:
    - Displays the first n rows of the DataFrame.
    - If `n` is not provided, it defaults to 5.
    - Here, `head(3)` shows the first 3 rows of `client_df`.
- <Output>
- With the client data, we have a mix of numeric and categorical data, which we will need to transform before modelling later.

#### Price Data:
```
price_df.head(3)
```
- <output>
- With the price data, it is purely numeric data but we can see a lot of zeros.

### III. Getting DataFrame Information
#### Client Data:
```
client_df.info()
```
- `info()` provides a summary of the DataFrame, including:
    - The number of entries (rows).
    - The number of columns.
    - The column names and their data types (e.g., integer, float, object).
    - The memory usage.
- <output>
- Output describes:
    - 26 columns and 14606 entries.
    - Example columns:
        - `id` (client company identifier) is an `object`.
        - `cons_12m` (electricity consumption of the past 12 months) is an `int64`.
        - `pow_max` (subscribed power) is a `float64`.
        - `chun` (has the client churned over the next 3 months) is an `int64`.
    - Totally:
        - `float64`: 11
        - `int64`: 7
        - `object`: 8
    - Memory usage: 2.9+ MB

#### Price Data:
```
price_df.info()
```
- <output>
- Output describes:
    - 8 columns and 193002 entries.
    - Example columns:
        - `id` (client company identifier) is an `object`.
        - `price_date` (reference date) is an `object`.
        - `price_off_peak_var` (price of energy for the 1st period (off peak)) is an `float64`.
    - Totally:
        - `float64`: 6
        - `object`: 2
    - Memory usage: 11.8+ MB
- You can see that all of the `datetime` related columns are not currently in `datetime` format. We will need to convert these later.

### IV. Statistical Summary
Now let's look at some statistics about the datasets:

#### Client Data
```
client_df.describe()
```
- `describe()` generates summary statistics for numerical columns.
- It includes:
    - Count (number of non-null values)
    - Mean (average value)
    - Standard deviation (`std`)
    - Min and Max
    - Percentiles (25%, 50% (median), 75%)
- <output>
- The describe method gives us a lot of information about the client data. The key point to take away from this is that we have highly skewed data, as exhibited by the percentile values.

#### Price Data
```
price_df.describe()
```
- <output>
- Overall the price data looks good.

## 3. Data visualization
Now let's dive a bit deeper into the dataframes:

### I. `plot_stacked_bars` Function
```
def plot_stacked_bars(dataframe, title_, size_=(18, 10), rot_=0, legend_="upper right"):
```
- This function plots a stacked bar chart using a pandas DataFrame.
- Parameters:
    - `dataframe`: The pandas DataFrame containing the data to be plotted.
    - `title_`: The title of the plot.
    - `size_`: The figure size (default is (`18,10`), meaning 18 inches wide and 10 inches tall).
    - `rot_`: Rotation angle for x-axis labels (default is `0`, meaning no rotation).
    - `legend_`: The position of the legend (default is `"upper right"`).

### II. Plotting the Stacked Bar Chart
```
ax = dataframe.plot(
    kind="bar",
    stacked=True,
    figsize=size_,
    rot=rot_,
    title=title_
)
```
- `dataframe.plot(kind="bar", stacked=True, figsize=size_, rot=rot_, title=title_)`
    - `kind="bar"` → Creates a bar chart.
    - `stacked=True` → Bars are stacked on top of each other instead of being side-by-side.
    - `figsize=size_` → Sets the figure size.
    - `rot=rot_` → Sets rotation for x-axis labels.
    - `title=title_` → Adds a title.
- The returned object `ax` is a matplotlib Axes object, which allows further customization of the plot.

### III. Adding Annotations
```
annotate_stacked_bars(ax, textsize=14)
```
- Calls the `annotate_stacked_bars` function (defined later) to add value labels to each bar.

### IV. Customizing the Legend
```
plt.legend(["Retention", "Churn"], loc=legend_)
```
- Renames the legend labels to `"Retention"` and `"Churn"`.
- `loc=legend_`: Places the legend at the specified location (default: `"upper right"`).

### V. Adding Labels & Displaying the Plot
```
plt.ylabel("Company base (%)")
plt.show()
```
- `plt.ylabel("Company base (%)")` → Labels the y-axis.
- `plt.show()` → Displays the plot.

### VI. `annotate_stacked_bars` Function
This function adds value labels to the stacked bars.
```
def annotate_stacked_bars(ax, pad=0.99, colour="white", textsize=13):
```
- Parameters:
    - `ax`: The matplotlib Axes object (the stacked bar chart).
    - `pad=0.99`: Slightly adjusts the position of the text labels.
    - `colour="white"`: Color of the text annotations (default: white).
    - `textsize=13`: Font size of the annotations.

### VII. Iterating Over Bars in the Plot
```
for p in ax.patches:
```
- `ax.patches` contains all the bars (rectangles) in the chart.

### VIII. Calculating and Adding Annotations
```
value = str(round(p.get_height(), 1))
```
- `p.get_height()` → Gets the height of the bar (i.e., the data value).
- `round(p.get_height(), 1)` → Rounds the value to 1 decimal place.
- `str(...)` → Converts the number to a string for displaying.

### IX. Skipping Zero Values
```
if value == '0.0':
    continue
```
- If the height of the bar is `0.0`, the function skips adding an annotation.

### X. Skipping Zero Values
```
ax.annotate(
    value,
    ((p.get_x()+ p.get_width()/2)*pad-0.05, (p.get_y()+p.get_height()/2)*pad),
    color=colour,
    size=textsize
)
```

### XI. Adding Text Annotations
```
ax.annotate(
    value,
    ((p.get_x()+ p.get_width()/2)*pad-0.05, (p.get_y()+p.get_height()/2)*pad),
    color=colour,
    size=textsize
)
```
- `ax.annotate(value, position, color=colour, size=textsize)` → Adds text on top of the bar.
- Position Calculation:
    - `p.get_x() + p.get_width()/2` → Centers the text horizontally.
    - `p.get_y() + p.get_height()/2` → Centers the text vertically.
    - `pad=0.99` adjusts the placement slightly.
    - `-0.05` fine-tunes the horizontal position.

### XII. Churn
Analyze customer retention vs churn rates in businesses.
- Extracts relevant columns (`id` and `churn`).
- Renames the columns for readability.
- Counts the number of companies in each churn category.
- Calculates the churn percentages.
- Plots a stacked bar chart to visualize the churn distribution.

#### Selecting Specific Columns
```
churn = client_df[['id', 'churn']]
```
- `client_df[['id', 'churn']]`:
    - Extracts only the `id` and `churn` columns from `client_df`.
    - Assumption:
        - `id`: Represents unique company/client identifiers.
        - `churn`: A categorical variable (e.g., `0` for retained clients and `1` for churned clients).
    - The new DataFrame `churn` now contains only these two columns.

#### Renaming Columns
```
churn.columns = ['Companies', 'churn']
```
- Renames the columns for better readability:
    - `id` → `Companies`
    - `churn` (remains the same)

#### Counting Companies by Churn Status
```
churn_total = churn.groupby(churn['churn']).count()
```
- `churn.groupby(churn['churn']).count()`:
    - Groups the DataFrame by the `churn` column.
    - Counts the number of companies (`Companies` column) in each churn category (`0` or `1`).

#### Calculating Churn Percentage
```
churn_percentage = churn_total / churn_total.sum() * 100
```
- Computes the percentage of churned and retained companies.
- Example Calculation:
    - Total Companies: 500 + 200 = 700
    - Retention Rate: 500 / 700 × 100 = 71.43%
    - Churn Rate: 200 / 700 × 100 = 28.57%

#### Plotting the Stacked Bar Chart
```
plot_stacked_bars(churn_percentage.transpose(), "Churning status", (5, 5), legend_="lower right")
```
- `churn_percentage.transpose()`:
    - Transposes the DataFrame to make it suitable for `plot_stacked_bars()`.
- `plot_stacked_bars(..., "Churning status", (5, 5), legend_="lower right")`:
    - Calls the `plot_stacked_bars` function (previously defined).
    - Title: `"Churning status"`.
    - Figure Size: `(5,5)`, meaning a smaller 5x5 inch chart.
    - Legend Position: `"lower right"`.
- Final visualisation:
    - This code creates a stacked bar chart showing the proportion of retained vs churned companies.
    - The legend in the lower right distinguishes the two groups.
- <output>
- About 10% of the total customers have churned. (This sounds about right)

### XIII. Sales Channel
This analysis helps businesses identify which sales channels have the highest customer churn, allowing them to target retention efforts effectively.
- Extracts relevant columns (`id`, `channel_sales`, `churn`).
- Groups data by `channel_sales` and `churn`, counting the number of companies per category.
- Reshapes data using `unstack()`, turning churn categories into columns.
- Computes churn percentages for each sales channel.
- Sorts the results to show the highest churn channels first.
- Outputs a table showing churn rates per sales channel.

#### Selecting Relevant Columns
```
channel = client_df[['id', 'channel_sales', 'churn']]
```
- Extracts only the `id`, `channel_sales`, and `churn` columns from `client_df` into a new DataFrame `channel`.
- Assumption:
    - `id`: Unique identifier for each client/company.
    - `channel_sales`: The sales channel through which the client was acquired (e.g., online, partner, direct sales).
    - `churn`: Whether the client has churned (`0` = retained, `1` = churned).

#### Grouping and Reshaping the Data
```
channel = channel.groupby([channel['channel_sales'], channel['churn']])['id'].count().unstack(level=1).fillna(0)
```
- Groups data by `channel_sales` (sales channel) and `churn` (retained or churned).
- Counts the number of unique `id`s (companies) for each combination of sales channel and churn status.
- This shows the number of retained and churned clients per sales channel.
- `unstack(level=1)`:
    - Moves the `churn` index from rows to columns.
    - Converts the grouped data into a table format where:
        - Rows = Sales channels (`channel_sales`).
        - Columns = Churn values (`0` for retained, `1` for churned).
- `fillna(0)`:
    - If any `channel_sales` category has no churned or retained clients, it fills missing values with `0`.


#### Calculating Churn Percentage and Sorting
```
channel_churn = (channel.div(channel.sum(axis=1), axis=0) * 100).sort_values(by=[1], ascending=False)
```
- `channel.sum(axis=1)`:
    - Sums the total clients (both churned and retained) per sales channel.
- `channel.div(channel.sum(axis=1), axis=0)`:
    - Divides each value in a row by the total number of clients in that row.
    - This converts raw counts into percentages.
- `* 100`:
    - Converts the proportion into a percentage.
- Sorts the table based on churn percentage (`1` column) in descending order.
- Channels with the highest churn appear first.

#### Displaying the Final Data
```
channel_churn
```
- Displays the final table, where:
    - Each row represents a sales channel.
    - Each column represents the percentage of retained (`0`) and churned (`1`) clients.
    - Sorted in descending order of churn rate.
- <output>
- Interestingly, the churning customers are distributed over 5 different values for `channel_sales`. As well as this, the value of `MISSING` has a churn rate of 7.6%. `MISSING` indicates a missing value and was added by the team when they were cleaning the dataset. This feature could be an important feature when it comes to building our model.

### XIV. Consumption
Let's see the distribution of the consumption in the last year and month. Since the consumption data is univariate, let's use histograms to visualize their distribution.
- Data Selection (`consumption`)
    - Extracts relevant features related to energy consumption and churn.
- Histogram Function (`plot_distribution`)
    - Creates stacked histograms to compare retained vs churned clients.
    - Uses 50 bins for smoother visualization.
- Histogram Subplots
    - Plots histograms for different consumption variables.
- Boxplot Subplots
    - Uses boxplots to visualize distributions and outliers.
- Formatting
    - Removes scientific notation and adjusts x-axis limits.

#### i. Extracting Relevant Data
```
consumption = client_df[['id', 'cons_12m', 'cons_gas_12m', 'cons_last_month', 'imp_cons', 'has_gas', 'churn']]
```
- This creates a new DataFrame `consumption` from `client_df`, selecting only specific columns related to energy consumption and churn:
    - `id`: Unique identifier for each company.
    - `cons_12m`: Total electricity consumption over the last 12 months.
    - `cons_gas_12m`: Total gas consumption over the last 12 months.
    - `cons_last_month`: Electricity consumption in the last month.
    - `imp_cons`: Estimated or imputed consumption (could be predicted or missing-value filled data).
    - `has_gas`: Indicates if the company has a gas connection (`'t'` for true, `'f'` for false).
    - `churn`: Whether the company churned or not (`0` = retained, `1` = churned).

#### ii. Defining the `plot_distribution` Function
```
def plot_distribution(dataframe, column, ax, bins_=50):
```
- This function plots a histogram of a specified column to compare retained (`churn=0`) vs. churned (`churn=1`) customers.

##### Creating a Temporary DataFrame
```
temp = pd.DataFrame({
    "Retention": dataframe[dataframe["churn"]==0][column],
    "Churn": dataframe[dataframe["churn"]==1][column]
})
```
- Creates a temporary DataFrame `temp`:
    - `Retention`: Extracts values of the given column for retained clients (`churn=0`).
    - `Churn`: Extracts values of the given column for churned clients (`churn=1`).

##### Plotting a Stacked Histogram
```
temp[["Retention","Churn"]].plot(kind='hist', bins=bins_, ax=ax, stacked=True)
```
- Plots a histogram where:
    - `kind='hist'`: Creates a histogram.
    - `bins=bins_`: Number of bins in the histogram (default = 50).
    - `ax=ax`: Uses the given subplot axis.
    - `stacked=True`: Stacks the two distributions on top of each other.

##### Formatting the Plot
```
ax.set_xlabel(column)
ax.ticklabel_format(style='plain', axis='x')
```
- Sets the x-axis label to the column name.
- Changes the x-axis format to remove scientific notation (`style='plain'`).

#### iii. Creating Subplots for Histograms
```
fig, axs = plt.subplots(nrows=4, figsize=(18, 25))
```
- Creates 4 subplots (one for each variable) in a figure of size 18x25 inches.
- `axs` is a list of Axes objects, where each element is used for one histogram.

##### Plotting Histograms
```
plot_distribution(consumption, 'cons_12m', axs[0])
plot_distribution(consumption[consumption['has_gas'] == 't'], 'cons_gas_12m', axs[1])
plot_distribution(consumption, 'cons_last_month', axs[2])
plot_distribution(consumption, 'imp_cons', axs[3])
```
- Calls `plot_distribution()` to plot histograms for different energy consumption variables:
    - `cons_12m`: Total electricity consumption over 12 months.
    - `cons_gas_12m` (only for clients with gas = `'t'`).
    - `cons_last_month`: Electricity consumption in the last month.
    - `imp_cons`: Estimated/imputed consumption.
- <output>
- Clearly, the consumption data is highly positively skewed, presenting a very long right-tail towards the higher values of the distribution.
- The values on the higher and lower end of the distribution are likely to be outliers.
- We can use a standard plot to visualise the outliers in more detail.
- A boxplot is a standardized way of displaying the distribution based on a five number summary:
    - Minimum
    - First quartile (Q1)
    - Median
    - Third quartile (Q3)
    - Maximum
- It can reveal outliers and what their values are. It can also tell us if our data is symmetrical, how tightly our data is grouped and if/how our data is skewed.

#### iv. Creating Boxplots
```
fig, axs = plt.subplots(nrows=4, figsize=(18,25))
```
- Creates another figure with 4 subplots for boxplots.

##### Plotting Boxplots
```
sns.boxplot(consumption["cons_12m"], ax=axs[0])
sns.boxplot(consumption[consumption["has_gas"] == "t"]["cons_gas_12m"], ax=axs[1])
sns.boxplot(consumption["cons_last_month"], ax=axs[2])
sns.boxplot(consumption["imp_cons"], ax=axs[3])
```
- Uses Seaborn's `boxplot` function to visualize the distribution of each variable.
- Boxplots help in identifying:
    - Median (middle line inside the box).
    - Interquartile range (IQR) (box size).
    - Outliers (dots outside the whiskers).

#### v. Formatting the Boxplots
```
for ax in axs:
    ax.ticklabel_format(style='plain', axis='x')
```
- Removes scientific notation from x-axis labels.
```
axs[0].set_xlim(-200000, 2000000)
axs[1].set_xlim(-200000, 2000000)
axs[2].set_xlim(-20000, 100000)
plt.show()
```
- Sets x-axis limits for better visualization.
- Displays the plots using `plt.show()`.
- <output>
- We will deal with skewness and outliers during feature engineering in the next exercise.

#### vi. Final Output
- Histograms: Show the distribution of energy consumption among retained and churned clients.
- Boxplots: Show median, spread, and outliers in consumption data.
- These visualizations help businesses identify differences in consumption patterns between retained and churned clients, aiding in customer behavior analysis and churn prediction.

### XV. Forecast
This analysis can help businesses adjust pricing strategies, offer better discounts, or predict which customers are at risk of churning based on their forecasted energy usage.
- Extracts forecast-related data into the `forecast` DataFrame.
- Creates subplots to display multiple histograms.
- Plots histograms to compare retained vs. churned customers based on forecasted energy consumption and pricing.
- Helps analyze how pricing, discounts, and forecasted consumption impact churn behavior.

#### Extracting Forecast-Related Data
```
forecast = client_df[
    ["id", "forecast_cons_12m",
    "forecast_cons_year","forecast_discount_energy","forecast_meter_rent_12m",
    "forecast_price_energy_off_peak","forecast_price_energy_peak",
    "forecast_price_pow_off_peak","churn"
    ]
]
```
- Creates a new DataFrame `forecast` by selecting columns related to forecasted energy consumption and pricing from `client_df`.
- Selected Columns:
    - `id` → Unique identifier for each client.
    - `forecast_cons_12m` → Forecasted electricity consumption for the next 12 months.
    - `forecast_cons_year` → Forecasted yearly consumption.
    - `forecast_discount_energy` → Forecasted energy discount (potential promotions).
    - `forecast_meter_rent_12m` → Forecasted meter rental cost for 12 months.
    - `forecast_price_energy_off_peak` → Forecasted price for off-peak energy usage.
    - `forecast_price_energy_peak` → Forecasted price for peak-time energy usage.
    - `forecast_price_pow_off_peak` → Forecasted price for off-peak power.
    - `churn` → Whether the client churned (`1`) or retained (`0`).
- This dataset helps analyze how forecasted energy usage and pricing affect customer retention and churn.

#### Creating Subplots for Histograms
```
fig, axs = plt.subplots(nrows=7, figsize=(18,50))
```
- Creates 7 subplots (one for each forecast-related variable except `id` and `churn`).
- `figsize=(18, 50)` ensures that the plots are large and easy to read.
- `axs` is a list of Axes objects, where each element corresponds to a separate subplot.

#### Plotting Histograms
```
plot_distribution(client_df, "forecast_cons_12m", axs[0])
plot_distribution(client_df, "forecast_cons_year", axs[1])
plot_distribution(client_df, "forecast_discount_energy", axs[2])
plot_distribution(client_df, "forecast_meter_rent_12m", axs[3])
plot_distribution(client_df, "forecast_price_energy_off_peak", axs[4])
plot_distribution(client_df, "forecast_price_energy_peak", axs[5])
plot_distribution(client_df, "forecast_price_pow_off_peak", axs[6])
```
- Calls the `plot_distribution function` (defined earlier) to plot histograms for each forecast-related variable.
- These histograms show the distribution of each forecast variable for retained (`churn=0`) and churned (`churn=1`) clients.
- <output>
- Similarly to the consumption plots, we can observe that a lot of the variables are highly positively skewed, creating a very long tail for the higher values.
- We will make some transformations during the next exercise to correct for this skewness.

#### What Each Histogram Represents
- Each stacked histogram visualizes how churned vs retained customers differ in forecasted data:

| Forecast Variable | Meaning | Insight from Histogram |
|-----------|-----------|----------------------|
| `forecast_cons_12m` | Forecasted electricity consumption in 12 months | Do churned customers have higher/lower forecasted consumption? |
| `forecast_cons_year` | Forecasted total yearly consumption | Does consumption prediction influence churn? |
| `forecast_discount_energy` | Forecasted discount on energy cost | Are churned customers receiving fewer discounts? |
| `forecast_meter_rent_12m` | Forecasted meter rent for 12 months | Do higher forecasted meter rents lead to more churn? |
| `forecast_price_energy_off_peak` | Forecasted price for off-peak energy usage | Are churned customers paying more for off-peak energy? |
| `forecast_price_energy_peak` | Forecasted price for peak energy usage | Are peak energy prices higher for churned customers? |
| `forecast_price_pow_off_peak` | Forecasted power price for off-peak usage | Do lower off-peak power prices reduce churn? |

- By analyzing these plots, patterns in customer churn behavior can be identified.

### XVI. Contract type
This analysis helps businesses understand the relationship between contract types and customer churn.
- Extracts contract and churn data from `client_df`.
- Groups and counts clients based on `has_gas` and `churn`.
- Converts counts to percentages to show churn distribution.
- Sorts by churn rate to identify which contract type has higher churn.
- Plots the results using `plot_stacked_bars`.

#### Extracting Relevant Data
```
contract_type = client_df[['id', 'has_gas', 'churn']]
```
- Creates a new DataFrame `contract_type` containing:
    - `id`: Unique client identifier.
    - `has_gas`: Whether the client has a gas contract (`'t'` for true, `'f'` for false).
    - `churn`: Whether the client churned (`1`) or retained (`0`).
- This dataset helps analyze how having a gas contract affects customer churn behavior.

#### Grouping and Counting Customers
```
contract = contract_type.groupby([contract_type['churn'], contract_type['has_gas']])['id'].count().unstack(level=0)
```
- Grouping by `churn` and `has_gas`:
    - This groups data based on whether the client has a gas contract (`has_gas`) and whether they churned (`churn`).
- Counting occurrences:
    - `['id'].count()` counts the number of clients in each group.
- Using `.unstack(level=0)`:
    - Converts the `churn` values (`0` = retained, `1` = churned) into separate columns.
    - This results in a DataFrame where:
        - Rows = Clients with (`'t'`) or without (`'f'`) gas contracts.
        - Columns = Retention (0) and Churn (1) counts.

#### Converting to Percentage
```
contract_percentage = (contract.div(contract.sum(axis=1), axis=0) * 100).sort_values(by=[1], ascending=False)
```
- Convert to percentage:
    - `contract.sum(axis=1)`: Computes the total number of clients for each `has_gas` category.
    - `div(..., axis=0)`: Divides each value by the total number of clients for that contract type.
    - `* 100`: Converts values to percentages.
- Sorting by churn percentage (`1`) in descending order:
    - This ensures that clients with the highest churn rate appear first.

#### Plotting the Data
```
plot_stacked_bars(contract_percentage, 'Contract type (with gas')
```
- Calls the `plot_stacked_bars` function to visualize the data.
- Generates a stacked bar chart, where:
    - The x-axis represents contract type (`'t'` or `'f'`).
    - The y-axis shows percentage distribution of churned and retained customers.
    - Colors differentiate churned vs retained clients.
- <output>
- The churn percentage for 't' (gas users) is lower than for 'f' (non-gas users) and the gas contracts may improve retention.
- 'f' (no gas) has a higher churn rate and the company should:
    - Offer gas contracts to reduce churn.
    - Investigate why non-gas clients are leaving.

### XVII. Margins
This analysis helps in understanding profitability and optimizing pricing strategies.
- Extracts margin-related data from `client_df`.
- Creates three subplots for gross margin, net margin on power, and overall net margin.
- Plots boxplots to visualize margin distribution and detect outliers.
- Formats x-axis labels to avoid scientific notation.
- Displays the figure with all boxplots.

#### Extracting Margin-Related Data
```
margin = client_df[['id', 'margin_gross_pow_ele', 'margin_net_pow_ele', 'net_margin']]
```
- Creates a new DataFrame `margin` with the following columns:
    - `id` → Unique identifier for each client.
    - `margin_gross_pow_ele` → Gross margin for electricity power.
    - `margin_net_pow_ele` → Net margin for electricity power.
    - `net_margin` → Overall net margin for the client.
- This data helps analyze profitability per client and identify patterns in margins.

#### Creating Subplots
```
fig, axs = plt.subplots(nrows=3, figsize=(18,20))
```
- Creates a figure (`fig`) and a set of subplots (`axs`).
- `nrows=3` → Three subplots (one for each margin-related variable).
- `figsize=(18,20)` → Ensures the plots are large and easy to read.
- `axs` is a list of three Axes objects, where:
    - `axs[0]` → First plot for `margin_gross_pow_ele`.
    - `axs[1]` → Second plot for `margin_net_pow_ele`.
    - `axs[2]` → Third plot for `net_margin`.

#### Plotting Boxplots
```
sns.boxplot(margin["margin_gross_pow_ele"], ax=axs[0])
sns.boxplot(margin["margin_net_pow_ele"],ax=axs[1])
sns.boxplot(margin["net_margin"], ax=axs[2])
```
- Uses Seaborn's `boxplot()` function to plot boxplots for each margin-related variable.
- Why Boxplots?
    - Shows the distribution of values (minimum, quartiles, median, outliers).
    - Helps detect anomalies (e.g., extreme outliers in margins).
- How Each Plot Works:
    - `axs[0]` → Boxplot for `margin_gross_pow_ele`.
    - `axs[1]` → Boxplot for `margin_net_pow_ele`.
    - `axs[2]` → Boxplot for `net_margin`.

#### Formatting the X-Axis
```
axs[0].ticklabel_format(style='plain', axis='x')
axs[1].ticklabel_format(style='plain', axis='x')
axs[2].ticklabel_format(style='plain', axis='x')
```
- Ensures x-axis values are displayed in plain format (not scientific notation).
- Why? Some margin values may be large, and scientific notation can be hard to read.

#### Displaying the Plots
```
plt.show()
```
- Displays the figure with all three boxplots.
- <output>
- We can see some outliers here as well which we will deal with in the next exercise.

### XVIII. Subscribed power
This analysis helps identify whether power consumption influences customer churn and guides retention strategies.
- Extracts power-related data (`id`, `pow_max`, `churn`).
- Creates a subplot for visualization.
- Plots a histogram to compare `pow_max` distribution for churned vs. retained clients.
- Analyzes power consumption trends to predict churn behavior.

#### Extracting Power-Related Data
```
power = client_df[['id', 'pow_max', 'churn']]
```
- Creates a new DataFrame `power` with the following columns:
    - `id` → Unique identifier for each client.
    - `pow_max` → Maximum power consumption recorded for the client.
    - `churn` → Indicates whether the client churned (`1`) or retained (`0`).

#### Creating a Subplot
```
fig, axs = plt.subplots(nrows=1, figsize=(18, 10))
```
- Creates a figure (`fig`) and one subplot (`axs`).
- `nrows=1` → Only one plot will be created.
- `figsize=(18, 10)` → Makes the plot large for better readability.
- `axs` is a single Axes object, used to plot the histogram.

#### Plotting the Distribution
```
plot_distribution(power, 'pow_max', axs)
```
- Calls the `plot_distribution` function, which was defined earlier, to:
    - Plot a stacked histogram of `pow_max`, separated by retained (`0`) vs. churned (`1`) clients.
    - Show how power consumption differs between churned and retained clients.
- <output>

### XIX. Other columns
This analysis helps identify key drivers of customer churn and informs retention strategies.
- Extracts relevant client data (`nb_prod_act`, `num_years_antig`, `origin_up`, `churn`).
- Groups and counts clients based on churn status for each factor.
- Converts counts into percentages for better interpretation.
- Visualizes the results using stacked bar charts.
- Provides actionable insights to reduce churn and improve retention strategies.

#### Extracting Relevant Data
```
others = client_df[['id', 'nb_prod_act', 'num_years_antig', 'origin_up', 'churn']]
```
- Creates a new DataFrame `others` with:
    - `id` → Unique identifier for each client.
    - `nb_prod_act` → Number of active products the client has.
    - `num_years_antig` → Number of years the client has been with the company (tenure).
    - `origin_up` → The origin of the client's contract or offer.
    - `churn` → Whether the client churned (`1`) or was retained (`0`).

#### Analyzing the Impact of Number of Products on Churn
```
products = others.groupby([others["nb_prod_act"],others["churn"]])["id"].count().unstack(level=1)
```
- Groups data by:
    - `nb_prod_act` (number of active products).
    - `churn` (retained `0` vs. churned `1`).
- Counts the number of clients in each group (`id.count()`).
- Unstacks `churn` so that:
    - One column represents retained clients (`0`).
    - Another column represents churned clients (`1`).

#### Converting to Percentage
```
products_percentage = (products.div(products.sum(axis=1), axis=0)*100).sort_values(by=[1], ascending=False)
```
- Calculates the churn percentage for each `nb_prod_act` group:
    - `products.sum(axis=1)` → Gets the total clients per product category.
    - `div(..., axis=0) * 100` → Converts values to percentages.
- Sorts by churn percentage (`1`) in descending order.
- Visualizing:
```
plot_stacked_bars(products_percentage, "Number of products")
```
- Calls `plot_stacked_bars` to generate a stacked bar chart, showing:
    - Churn percentage vs. Number of products.

#### Analyzing the Impact of Client Tenure on Churn
```
years_antig = others.groupby([others["num_years_antig"],others["churn"]])["id"].count().unstack(level=1)
years_antig_percentage = (years_antig.div(years_antig.sum(axis=1), axis=0)*100)
plot_stacked_bars(years_antig_percentage, "Number years")
```
- Groups clients by `num_years_antig` (years of tenure) and churn status.
- Calculates churn percentages for each tenure group.
- Visualizes churn trends across different tenure lengths.

#### Analyzing the Impact of Contract/Offer Origin on Churn
```
origin = others.groupby([others["origin_up"],others["churn"]])["id"].count().unstack(level=1)
origin_percentage = (origin.div(origin.sum(axis=1), axis=0)*100)
plot_stacked_bars(origin_percentage, "Origin contract/offer")
```
- Groups clients by `origin_up` (contract/offer origin) and churn status.
- Calculates churn percentages for different contract origins.
- Visualizes churn rates for different acquisition sources.


---


