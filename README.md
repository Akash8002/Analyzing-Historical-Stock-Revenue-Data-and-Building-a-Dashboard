# Analyzing Historical Stock Revenue Data and Building a Dashboard

## Description
This project focuses on extracting and visualizing stock data to support decision-making processes. By extracting stock data using `yfinance` and revenue data through web scraping, we aim to generate insightful graphs that display the relationship between stock prices and revenues for companies like Tesla and GameStop. These visualizations help users understand historical stock trends and revenue fluctuations.

## Table of Contents
- [Define a Function that Makes a Graph](#define-a-function-that-makes-a-graph)
- [Question 1: Use yfinance to Extract Stock Data](#question-1-use-yfinance-to-extract-stock-data)
- [Question 2: Use Webscraping to Extract Tesla Revenue Data](#question-2-use-webscraping-to-extract-tesla-revenue-data)
- [Question 3: Use yfinance to Extract Stock Data](#question-3-use-yfinance-to-extract-stock-data)
- [Question 4: Use Webscraping to Extract GME Revenue Data](#question-4-use-webscraping-to-extract-gme-revenue-data)
- [Question 5: Plot Tesla Stock Graph](#question-5-plot-tesla-stock-graph)
- [Question 6: Plot GameStop Stock Graph](#question-6-plot-gamestop-stock-graph)

## Estimated Time Needed: 30 min

---

### Note:
If you are working locally using Anaconda, please uncomment the following code and execute it. Use the version as per your Python version.

```bash
%pip install yfinance
%pip install bs4
%pip install nbformat
```

### Required Libraries
The following libraries are required for this project:
- `yfinance` for extracting stock data.
- `bs4` (BeautifulSoup) for web scraping revenue data.
- `nbformat` for working with Jupyter notebooks.
- `pandas` for handling data.
- `plotly` for creating visualizations.

### Installation:
Make sure you have the following libraries installed:

```bash
pip install yfinance pandas requests beautifulsoup4 plotly
```

### Step 1: Import Libraries

```python
import yfinance as yf
import pandas as pd
import requests
from bs4 import BeautifulSoup
import plotly.graph_objects as go
from plotly.subplots import make_subplots
```

---

### Define Graphing Function
The `make_graph` function is responsible for plotting the stock prices and revenues on a dual-axis graph.

```python
def make_graph(stock_data, revenue_data, stock):
    fig = make_subplots(rows=2, cols=1, shared_xaxes=True, subplot_titles=("Historical Share Price", "Historical Revenue"), vertical_spacing=.3)
    stock_data_specific = stock_data[stock_data.Date <= '2021-06-14']
    revenue_data_specific = revenue_data[revenue_data.Date <= '2021-04-30']
    
    fig.add_trace(go.Scatter(x=pd.to_datetime(stock_data_specific.Date, infer_datetime_format=True), y=stock_data_specific.Close.astype("float"), name="Share Price"), row=1, col=1)
    fig.add_trace(go.Scatter(x=pd.to_datetime(revenue_data_specific.Date, infer_datetime_format=True), y=revenue_data_specific.Revenue.astype("float"), name="Revenue"), row=2, col=1)

    fig.update_xaxes(title_text="Date", row=1, col=1)
    fig.update_xaxes(title_text="Date", row=2, col=1)
    fig.update_yaxes(title_text="Price ($US)", row=1, col=1)
    fig.update_yaxes(title_text="Revenue ($US Millions)", row=2, col=1)

    fig.update_layout(showlegend=False,
                     height=900,
                     title=stock,
                     xaxis_rangeslider_visible=True)
    fig.show()
```

---

### Question 1: Use yfinance to Extract Stock Data
Using `yfinance`, you can extract stock data for Tesla (`TSLA`) and display the results.

```python
tesla = yf.Ticker("TSLA")
tesla_data = tesla.history(period="max")
tesla_data.reset_index(inplace=True)
tesla_data.head(5)
```

**Sample Output:**

| Date                       | Open   | High   | Low    | Close  | Volume     | Dividends | Stock Splits |
|----------------------------|--------|--------|--------|--------|------------|-----------|--------------|
| 2010-06-29 00:00:00-04:00  | 1.27   | 1.67   | 1.17   | 1.59   | 281494500  | 0.0       | 0.0          |
| 2010-06-30 00:00:00-04:00  | 1.72   | 2.03   | 1.55   | 1.59   | 257806500  | 0.0       | 0.0          |
| ...                        | ...    | ...    | ...    | ...    | ...        | ...       | ...          |

---

### Question 2: Use Webscraping to Extract Tesla Revenue Data
Use `requests` to fetch Tesla's revenue data from a webpage and `BeautifulSoup` to parse the HTML content.

```python
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/revenue.htm"
response = requests.get(url)
html_data = response.text

soup = BeautifulSoup(html_data, 'html.parser')
```

---

### Questions 3, 4, 5, 6
The same procedure is followed for extracting and visualizing data for GameStop (`GME`) and plotting graphs for both companies' stock prices and revenue data.
