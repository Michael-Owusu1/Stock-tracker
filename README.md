# Stock Trend Analyser

A Python command-line app that lets you search for a company, pull its historical stock prices, and see whether it's trending up or down — with a chart to match.

## What it does

- **Search by company name** — no need to already know the exact ticker symbol. Type "Apple" or "Tesco" and pick the right listing from a numbered list (handy since many companies are listed on multiple exchanges).
- **Fetch historical daily prices** via the [Alpha Vantage](https://www.alphavantage.co/) API.
- **Save the data to CSV** so you're not stuck re-calling the API every time you want to look at it again.
- **Calculate a configurable moving average** (you choose the number of days) to smooth out daily noise and reveal the underlying trend.
- **Print a plain-English trend summary** — is the price currently above or below its recent average?
- **Plot the trend** with matplotlib, showing the closing price against its moving average.

## Example

```
Enter a company: Tesco
1. TSCO.LON - Tesco PLC, United Kingdom
2. TSCO.MEX - Tesco PLC, Mexico

Enter the number of the company you are looking for: 1
Enter the amount of days you would like to see for the moving average: 30

The data has been saved to this file: TSCO.LON.csv

 TSCO.LON Trend Summary
The Latest Closing Price: 461.20
30 day/s Moving Average: 472.60
There is a downwards trend!! Closing Price is BELOW the moving average.
```

...followed by a chart showing the closing price line against the moving average line.

## Tech used

- **Python**
- **[Requests](https://docs.python-requests.org/)** — for calling the Alpha Vantage API
- **[Pandas](https://pandas.pydata.org/)** — for cleaning and structuring the price data, and calculating the rolling moving average
- **[Matplotlib](https://matplotlib.org/)** — for plotting the trend chart
- **[python-dotenv](https://pypi.org/project/python-dotenv/)** — for keeping the API key out of the source code

## Setup

**1. Clone the repo**
```bash
git clone https://github.com/Michael-Owusu1/Stock-tracker-analyser.git
cd Stock-tracker-analyser
```

**2. Install dependencies**
```bash
pip install requests pandas matplotlib python-dotenv
```

**3. Get a free Alpha Vantage API key**

Sign up at [alphavantage.co/support/#api-key](https://www.alphavantage.co/support/#api-key).

**4. Add your API key**

Create a file called `.env` in the project folder with:
```
api_key=your_actual_key_here
```

**5. Run it**
```bash
python stock_tracker.py
```

## How it works

1. `search_company()` — searches Alpha Vantage's `SYMBOL_SEARCH` endpoint for companies matching what you typed.
2. `display_search()` — shows a numbered list of matches so you can pick the exact listing you want (important, since the same company can be listed on multiple exchanges).
3. `get_stock()` — fetches historical daily price data for your chosen ticker from the `TIME_SERIES_DAILY` endpoint.
4. `csv_()` — cleans the raw JSON into a proper table (using pandas), sorts it chronologically, and saves it to a CSV file.
5. `calculate_moving_average()` — adds a rolling average column based on however many days you choose.
6. `display_trends()` — prints a plain-English summary comparing the latest price to its moving average.
7. `plot_trends()` — draws the closing price and moving average on a chart.

## Notes on scope

This project calculates and visualizes **historical trends** — it does not predict future prices. Predicting stock movement is a hard problem to solve reliably, so this tool sticks to describing what's already happened rather than claiming to forecast what's next.

## Limitations

- Alpha Vantage's free tier allows **25 requests/day** and **5 requests/minute** — heavy testing can hit this limit.
- Search coverage is stronger for US-listed companies than some international exchanges.

## Possible future improvements

- Add a simple web interface
- Compare multiple stocks side by side on one chart
- Support weekly/monthly time series in addition to daily
