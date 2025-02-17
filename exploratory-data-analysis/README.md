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
- ax.annotate(value, position, color=colour, size=textsize) → Adds text on top of the bar.
- Position Calculation:
    - `p.get_x() + p.get_width()/2` → Centers the text horizontally.
    - `p.get_y() + p.get_height()/2` → Centers the text vertically.
    - `pad=0.99` adjusts the placement slightly.
    - `-0.05` fine-tunes the horizontal position.











### Churn
```
churn = client_df[['id', 'churn']]
churn.columns = ['Companies', 'churn']
churn_total = churn.groupby(churn['churn']).count()
churn_percentage = churn_total / churn_total.sum() * 100
```

```
plot_stacked_bars(churn_percentage.transpose(), "Churning status", (5, 5), legend_="lower right")
```

About 10% of the total customers have churned. (This sounds about right)

### Sales channel
```
channel = client_df[['id', 'channel_sales', 'churn']]
channel = channel.groupby([channel['channel_sales'], channel['churn']])['id'].count().unstack(level=1).fillna(0)
channel_churn = (channel.div(channel.sum(axis=1), axis=0) * 100).sort_values(by=[1], ascending=False)
```

```
channel_churn
```

Interestingly, the churning customers are distributed over 5 different values for `channel_sales`. As well as this, the value of `MISSING` has a churn rate of 7.6%. `MISSING` indicates a missing value and was added by the team when they were cleaning the dataset. This feature could be an important feature when it comes to building our model.

### Consumption

Let's see the distribution of the consumption in the last year and month. Since the consumption data is univariate, let's use histograms to visualize their distribution.
```
consumption = client_df[['id', 'cons_12m', 'cons_gas_12m', 'cons_last_month', 'imp_cons', 'has_gas', 'churn']]
```

```
def plot_distribution(dataframe, column, ax, bins_=50):
    """
    Plot variable distribution in a stacked histogram of churned or retained company
    """
    # Create a temporal dataframe with the data to be plot
    temp = pd.DataFrame({"Retention": dataframe[dataframe["churn"]==0][column],
    "Churn":dataframe[dataframe["churn"]==1][column]})
    # Plot the histogram
    temp[["Retention","Churn"]].plot(kind='hist', bins=bins_, ax=ax, stacked=True)
    # X-axis label
    ax.set_xlabel(column)
    # Change the x-axis to plain style
    ax.ticklabel_format(style='plain', axis='x')
```

```
fig, axs = plt.subplots(nrows=4, figsize=(18, 25))

plot_distribution(consumption, 'cons_12m', axs[0])
plot_distribution(consumption[consumption['has_gas'] == 't'], 'cons_gas_12m', axs[1])
plot_distribution(consumption, 'cons_last_month', axs[2])
plot_distribution(consumption, 'imp_cons', axs[3])
```

Clearly, the consumption data is highly positively skewed, presenting a very long right-tail towards the higher values of the distribution. The values on the higher and lower end of the distribution are likely to be outliers. We can use a standard plot to visualise the outliers in more detail. A boxplot is a standardized way of displaying the distribution based on a five number summary:
- Minimum
- First quartile (Q1)
- Median
- Third quartile (Q3)
- Maximum

It can reveal outliers and what their values are. It can also tell us if our data is symmetrical, how tightly our data is grouped and if/how our data is skewed.

```
fig, axs = plt.subplots(nrows=4, figsize=(18,25))

# Plot histogram
sns.boxplot(consumption["cons_12m"], ax=axs[0])
sns.boxplot(consumption[consumption["has_gas"] == "t"]["cons_gas_12m"], ax=axs[1])
sns.boxplot(consumption["cons_last_month"], ax=axs[2])
sns.boxplot(consumption["imp_cons"], ax=axs[3])

# Remove scientific notation
for ax in axs:
    ax.ticklabel_format(style='plain', axis='x')
    # Set x-axis limit
    axs[0].set_xlim(-200000, 2000000)
    axs[1].set_xlim(-200000, 2000000)
    axs[2].set_xlim(-20000, 100000)
    plt.show()
```

We will deal with skewness and outliers during feature engineering in the next exercise.

### Forecast

```
forecast = client_df[
    ["id", "forecast_cons_12m",
    "forecast_cons_year","forecast_discount_energy","forecast_meter_rent_12m",
    "forecast_price_energy_off_peak","forecast_price_energy_peak",
    "forecast_price_pow_off_peak","churn"
    ]
]
```

```
fig, axs = plt.subplots(nrows=7, figsize=(18,50))

# Plot histogram
plot_distribution(client_df, "forecast_cons_12m", axs[0])
plot_distribution(client_df, "forecast_cons_year", axs[1])
plot_distribution(client_df, "forecast_discount_energy", axs[2])
plot_distribution(client_df, "forecast_meter_rent_12m", axs[3])
plot_distribution(client_df, "forecast_price_energy_off_peak", axs[4])
plot_distribution(client_df, "forecast_price_energy_peak", axs[5])
plot_distribution(client_df, "forecast_price_pow_off_peak", axs[6])
```

Similarly to the consumption plots, we can observe that a lot of the variables are highly positively skewed, creating a very long tail for the higher values. We will make some transformations during the next exercise to correct for this skewness.

### Contract type

```
contract_type = client_df[['id', 'has_gas', 'churn']]
contract = contract_type.groupby([contract_type['churn'], contract_type['has_gas']])['id'].count().unstack(level=0)
contract_percentage = (contract.div(contract.sum(axis=1), axis=0) * 100).sort_values(by=[1], ascending=False)
```

```
plot_stacked_bars(contract_percentage, 'Contract type (with gas')
```

### Margins
```
margin = client_df[['id', 'margin_gross_pow_ele', 'margin_net_pow_ele', 'net_margin']]
```

```
fig, axs = plt.subplots(nrows=3, figsize=(18,20))
# Plot histogram
sns.boxplot(margin["margin_gross_pow_ele"], ax=axs[0])
sns.boxplot(margin["margin_net_pow_ele"],ax=axs[1])
sns.boxplot(margin["net_margin"], ax=axs[2])
# Remove scientific notation
axs[0].ticklabel_format(style='plain', axis='x')
axs[1].ticklabel_format(style='plain', axis='x')
axs[2].ticklabel_format(style='plain', axis='x')
plt.show()
```

We can see some outliers here as well which we will deal with in the next exercise.

### Subscribed power
```
power = client_df[['id', 'pow_max', 'churn']]
```

```
fig, axs = plt.subplots(nrows=1, figsize=(18, 10))
plot_distribution(power, 'pow_max', axs)
```

### Other columns

```
others = client_df[['id', 'nb_prod_act', 'num_years_antig', 'origin_up', 'churn']]
products = others.groupby([others["nb_prod_act"],others["churn"]])["id"].count().unstack(level=1)
products_percentage = (products.div(products.sum(axis=1), axis=0)*100).sort_values(by=[1], ascending=False)
```

```
plot_stacked_bars(products_percentage, "Number of products")
```

```
years_antig = others.groupby([others["num_years_antig"],others["churn"]])["id"].count().unstack(level=1)
years_antig_percentage = (years_antig.div(years_antig.sum(axis=1), axis=0)*100)
plot_stacked_bars(years_antig_percentage, "Number years")
```

```
origin = others.groupby([others["origin_up"],others["churn"]])["id"].count().unstack(level=1)
origin_percentage = (origin.div(origin.sum(axis=1), axis=0)*100)
plot_stacked_bars(origin_percentage, "Origin contract/offer")
```



---


