# Shopify Stock Data Analysis

## Project Overview

This project performs a basic analysis of Shopify stock market data
using Python. The program reads stock data from a CSV file, checks and
describes the dataset, converts and sorts the date values, visualizes
stock prices and trading volume, calculates moving averages, daily
returns, and rolling volatility.

The source program was originally created in Google Colab. It loads the
dataset from `shopify_stock.csv`.

## Technologies Used

-   Python
-   Pandas
-   Matplotlib
-   Seaborn
-   Google Colab

## Libraries Used

``` python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

### 1. Pandas

Pandas is used for reading, processing, sorting, and analyzing the stock
dataset.

### 2. Matplotlib

Matplotlib is used to create graphs for stock prices, stock volume,
daily returns, and other analysis.

### 3. Seaborn

Seaborn is imported in the program, but no Seaborn function is used in
the shown code.

------------------------------------------------------------------------

## Dataset Loading

``` python
df = pd.read_csv("/content/shopify_stock.csv")
```

### Explanation

`pd.read_csv()` reads the CSV file and stores the data in a Pandas
DataFrame called `df`.

-   `df` = DataFrame containing the stock data
-   `shopify_stock.csv` = input dataset

The dataset is expected to contain columns such as:

-   `date`
-   `open`
-   `high`
-   `low`
-   `close`
-   `volume`

------------------------------------------------------------------------

# Functions and Operations Explained

## 1. `df.head()`

``` python
df.head()
```

### Function

Displays the first five rows of the dataset by default.

### Purpose

It is useful for quickly checking whether the data has been loaded
correctly.

------------------------------------------------------------------------

## 2. `df.info()`

``` python
df.info()
```

### Function

Displays information about the DataFrame.

### It shows

-   Number of rows
-   Column names
-   Data types
-   Non-null values
-   Memory usage

### Purpose

It helps us understand the structure and quality of the dataset.

------------------------------------------------------------------------

## 3. `df.describe()`

``` python
df.describe()
```

### Function

Provides statistical information about numerical columns.

### It includes

-   Count
-   Mean
-   Standard deviation
-   Minimum
-   25% value
-   50% value
-   75% value
-   Maximum

### Purpose

It helps understand the basic statistics of stock prices and volume.

------------------------------------------------------------------------

## 4. Convert Date to DateTime

``` python
df['date'] = pd.to_datetime(df['date'], utc=True)
```

### Function

`pd.to_datetime()` converts the `date` column into a proper date/time
format.

### `utc=True`

It makes the date values timezone-aware using UTC.

### Purpose

DateTime format makes it easier to sort and work with dates for
time-series analysis.

------------------------------------------------------------------------

## 5. Check Date Data Type

``` python
df["date"].dtype
```

### Function

Displays the data type of the `date` column.

### Purpose

It confirms that the date column has been converted to a DateTime type.

------------------------------------------------------------------------

## 6. Sort Data by Date

``` python
df = df.sort_values("date")
```

### Function

Sorts the DataFrame according to the `date` column.

### Purpose

Stock data is time-series data, so arranging records chronologically is
important before performing time-based calculations.

------------------------------------------------------------------------

# Stock Price Visualization

``` python
plt.figure(figsize=(10,5))
plt.plot(df.date, df["open"], label="open")
plt.plot(df.date, df["high"], label="high")
plt.plot(df.date, df["low"], label="low")
plt.plot(df.date, df["close"], label="close")
```

### Explanation

`plt.figure(figsize=(10,5))` creates a graph with a specified size.

`plt.plot()` creates line graphs.

Four stock price values are plotted:

-   Open price
-   High price
-   Low price
-   Close price

The `label` parameter gives each line a name for the legend.

### Graph Labels

``` python
plt.title("stock price")
plt.xlabel("date")
plt.ylabel("price")
plt.xticks(rotation=45)
plt.legend()
```

-   `plt.title()` gives the graph a title.
-   `plt.xlabel()` labels the X-axis.
-   `plt.ylabel()` labels the Y-axis.
-   `plt.xticks(rotation=45)` rotates date labels by 45 degrees.
-   `plt.legend()` displays the labels of the plotted lines.

------------------------------------------------------------------------

# Stock Volume Visualization

``` python
plt.figure(figsize=(10,5))
plt.plot(df["date"], df["volume"])
```

### Function

Creates a line graph showing stock trading volume over time.

``` python
plt.title("stock volume")
plt.xlabel("date")
plt.ylabel("volume")
plt.xticks(rotation=45)
```

These statements add the title and axis labels.

### Purpose

This graph helps visualize how trading volume changes over different
dates.

------------------------------------------------------------------------

# Moving Averages

``` python
df["MA20"] = df["close"].rolling(20).mean()
df["MA50"] = df["close"].rolling(50).mean()
df["MA200"] = df["close"].rolling(200).mean()
```

### What is a Moving Average?

A moving average calculates the average value over a specific number of
previous data points.

### MA20

``` python
df["MA20"] = df["close"].rolling(20).mean()
```

Calculates the 20-period moving average of the closing price.

### MA50

``` python
df["MA50"] = df["close"].rolling(50).mean()
```

Calculates the 50-period moving average.

### MA200

``` python
df["MA200"] = df["close"].rolling(200).mean()
```

Calculates the 200-period moving average.

### Purpose

Moving averages can be used to observe the general trend of stock prices
by smoothing short-term price changes.

------------------------------------------------------------------------

# Daily Return

``` python
df["Daily_return"] = df["close"].pct_change()
```

### Function

`pct_change()` calculates the percentage change between the current
closing price and the previous closing price.

Conceptually:

``` text
Daily Return = (Current Close - Previous Close) / Previous Close
```

### Example

If yesterday's closing price is 100 and today's closing price is 105:

``` text
Daily Return = (105 - 100) / 100
             = 0.05
             = 5%
```

### Purpose

Daily return shows how much the stock price changed from one trading
period to the next.

The first row normally has no previous value, so its percentage change
is missing (`NaN`).

------------------------------------------------------------------------

# Daily Return Visualization

``` python
plt.figure(figsize=(10,5))
plt.plot(df["Daily_return"])
```

This creates a line graph of daily returns.

``` python
plt.title("Daily return")
plt.xlabel("Daliy return")
plt.ylabel("Frequency")
plt.show()
```

These statements add labels and display the graph.

Note: the source code labels the axes as `Daliy return` and `Frequency`;
these are simply the labels used in the original program.

------------------------------------------------------------------------

# Rolling Volatility

``` python
df["Rolling_Volatility_20"] = (
    df["Daily_return"].rolling(window=20).std()
)
```

### Function

This calculates the standard deviation of daily returns over a rolling
20-period window.

### Important Functions

#### `rolling(window=20)`

Creates a moving window containing 20 observations.

#### `.std()`

Calculates the standard deviation within each window.

### What is Volatility?

Volatility describes how much the stock's returns vary over time.

-   Higher volatility means larger variations in returns.
-   Lower volatility means smaller variations in returns.

The program stores the result in:

``` python
Rolling_Volatility_20
```

------------------------------------------------------------------------

## `dropna()`

``` python
df["Rolling_Volatility_20"].dropna()
```

### Function

Removes missing values from the selected volatility series.

### Why are there missing values?

A 20-period rolling calculation needs enough previous observations.
Therefore, the first part of the series does not have a complete
20-period window.

------------------------------------------------------------------------

# Overall Workflow

``` text
Load CSV Data
      ↓
Inspect Dataset
      ↓
Convert Date Column
      ↓
Sort Data by Date
      ↓
Visualize Stock Prices
      ↓
Visualize Stock Volume
      ↓
Calculate MA20, MA50, MA200
      ↓
Calculate Daily Returns
      ↓
Calculate 20-Period Rolling Volatility
```

# Important Note About the Source Code

The provided program contains this statement:

``` python
plt.plot("Shopify Stock Volume")
```

This appears in the moving-average plotting section. Unlike the other
`plt.plot()` calls, it passes a text string rather than a numerical data
series. Depending on the Matplotlib version and execution context, this
may not produce the intended moving-average chart and may cause an
error.

Also, although `seaborn` is imported, no Seaborn function is used in the
provided code.

These notes describe the supplied source code rather than changing it.

# Conclusion

This project demonstrates basic time-series stock analysis using Pandas
and Matplotlib. It covers dataset inspection, date processing,
stock-price visualization, trading-volume visualization, moving
averages, daily returns, and rolling volatility. These operations
provide a basic understanding of how stock data can be explored and
analyzed using Python.
