# Yahooquery
[![CodeFactor](https://www.codefactor.io/repository/github/dpguthrie/yahooquery/badge/master?s=289f5ed067de511ac29b5e229c1a5ef5c8c1dc83)](https://www.codefactor.io/repository/github/dpguthrie/yahooquery/overview/master)
[![PyPi downloads](https://pypip.in/d/yahooquery/badge.png)](https://crate.io/packages/yahooquery/)
[![PyPI version shields.io](https://img.shields.io/pypi/v/yahooquery.svg)](https://pypi.python.org/pypi/yahooquery/)
[![PyPI license](https://img.shields.io/pypi/l/yahooquery.svg)](https://pypi.python.org/pypi/yahooquery/)
[![PyPI pyversions](https://img.shields.io/pypi/pyversions/yahooquery.svg)](https://pypi.python.org/pypi/yahooquery/)
[![Build Status](https://travis-ci.com/dpguthrie/yahooquery.svg?branch=master)](https://travis-ci.com/dpguthrie/yahooquery)
[![codecov](https://codecov.io/gh/dpguthrie/yahooquery/branch/master/graph/badge.svg)](https://codecov.io/gh/dpguthrie/yahooquery)

Python wrapper around an unofficial Yahoo Finance API.  Check out an interactive demo at (https://yahooquery-streamlit.herokuapp.com)

## 2.0.0 UPDATES

- Yahoo Finance Premium data (for subscribed users)
- Option to make asynchronous and synchronous requests
- Faster option data retrieval
- **EVEN MORE DATA**

## Install

```bash
pip install yahooquery
```

## Ticker

The `Ticker` module is the access point to the Yahoo Finance API.  Pass a ticker symbol to the `Ticker` class.

```python
from yahooquery import Ticker

aapl = Ticker('aapl')
```

Or pass a list of tickers.

```python
tickers = Ticker(['aapl', 'msft'])

# is equivalent to
tickers = Ticker('aapl msft')

# is equivalent to
tickers = Ticker('aapl, msft')
```

### New to 2.0.0

Additional keyword arguments can be passed to the class to modify certain behavior:
- `asynchronous`:  Pass `asynchronous=True` and requests made with multiple symbols will be made asynchronously.  Default is `False`
- `max_workers`:  Pass `max_workers=<n>` and modify how many workers are available to make asynchronous requests.  This is only used when `asynchronous=True` is passed as well.  Default is `8`
- `proxies`:  Pass `proxies={'http': ..., 'https': ...}` to use a proxy when making a request.  This is **recommended** when making asynchronous requests.
- `formatted`: Pass `formatted=True` to receive most numeric data in the following form:  `'price': {'raw': 126000000000, 'fmt': '$126B'}`  Default is `False`
- `username` and `password`:  If you subscribe to Yahoo Finance Premium, pass your `username` and `password`.  You will be logged in and will now be able to access premium properties / methods.  All premium properties / methods begin with `p_`.  **You do not need to be logged in to access all other properties and methods.**

## Data

Based on the data you'd like, the result will either be accessed through a `dict` or as a `pandas.DataFrame`.  Accessing data is incredibly easy and pythonic.

### Dictionaries

```python
aapl = Ticker('aapl')

# Asset Profile
aapl.asset_profile
{'aapl': {'address1': 'One Apple Park Way', 'city': 'Cupertino', ... }}

# ESG Scores
aapl.esg_scores
{'aapl': {'totalEsg': 72.27, 'environomentScore': 89.81, ... }}

# Financial Data
aapl.financial_data
{'aapl': {'currentPrice': 275.15, 'targetHighPrice': 342.4, ... }}

# Key Statistics
aapl.key_stats
{'aapl': {'priceHint': 2, 'enterpriseValue': 1230054359040, ... }}

# Price Information
aapl.price
{'aapl': {'preMarketChange': {}, 'preMarketPrice': {}, ... }}

# Quote Type
aapl.quote_type
{'aapl': {'exchange': 'NMS', 'quoteType': 'EQUITY', ... }}

# Share Purchase Activity
aapl.share_purchase_activity
{'aapl': {'period': '6m', 'buyInfoCount': 20, ... }}

# Summary Information
aapl.summary_detail
{'aapl': {'priceHint': 2, 'previousClose': 271.46, ... }}
aapl.summary_profile
{'aapl': {'address1': 'One Apple Park Way', 'city': 'Cupertino', ... }}
```

How about more than one ticker?

```python
# Pass a list of tickers to the Ticker class
tickers = Ticker('aapl msft')

tickers.asset_profile
{'aapl': {'address1': 'One Apple Park Way', 'city': 'Cupertino', ... }, 'msft': {'address1': 'One Microsoft Way', 'city': 'Redmond', ... }}

tickers.esg_scores
{'aapl': {'totalEsg': 72.27, 'environomentScore': 89.81, ... }, 'msft': {'totalEsg': 74.8, 'environmentScore': 84.17, ... }}

tickers.financial_data
{'aapl': {'currentPrice': 275.15, 'targetHighPrice': 342.4, ... }, 'msft': {'currentPrice': 154.53, 'targetHighPrice': 174.0, ... }}

tickers.key_stats
{'aapl': {'priceHint': 2, 'enterpriseValue': 1230054359040, ... }, 'msft': {'priceHint': 2, 'enterpriseValue': 1127840350208, ... }}

tickers.price
{'aapl': {'preMarketChange': {}, 'preMarketPrice': {}, ... }, 'msft': {'preMarketChange': {}, 'preMarketPrice': {}, ... }}

tickers.quote_type
{'aapl': {'exchange': 'NMS', 'quoteType': 'EQUITY', ... }, 'msft': {'exchange': 'NMS', 'quoteType': 'EQUITY', ... }}

tickers.share_purchase_activity
{'aapl': {'period': '6m', 'buyInfoCount': 20, ... }, 'msft': {'period': '6m', 'buyInfoCount': 30, ... }}

tickers.summary_detail
{'aapl': {'priceHint': 2, 'previousClose': 271.46, ... }, 'msft': {'priceHint': 2, 'previousClose': 153.24, ... }}

tickers.summary_profile
{'aapl': {'address1': 'One Apple Park Way', 'city': 'Cupertino', ... }, 'msft': {'address1': 'One Microsoft Way', 'city': 'Redmond', ... }}

```

### New in 2.0.0

```python
# News Articles
aapl.news

# Trend data related to a symbols page views
aapl.page_views

# Top 5 recommended symbols based on a symbol(s)
aapl.recommendations

# Technical trading insights
aapl.technical_insights

# Validate symbol's existence
aapl.validation
```

### Dataframes

```python
aapl.company_officers
aapl.earning_history
aapl.grading_history
aapl.insider_holders
aapl.insider_transactions
aapl.institution_ownership
aapl.recommendation_trend
aapl.sec_filings
aapl.fund_ownership
aapl.major_holders
aapl.earnings_trend

# The following methods take a frequency argument.  If nothing is provided, annual data will be returned.  To return quarterly data, pass "q" as an argument.
aapl.balance_sheet()  # Defaults to Annual
aapl.balance_sheet(frequency="q")
aapl.balance_sheet("q")
aapl.cash_flow()
aapl.income_statement()
```

## Premium (New in 2.0.0)

### Login

If you subscribe to Yahoo Finance Premium, you can utilize this package to retrieve premium data as well.  You can pass your login credentials (username and password) when you initialize the `Ticker` class:

```python
tickers = Ticker('aapl msft fb', username='my_email@gmail.com', password='my_password')
```

Or you can login after initializing the `Ticker` class:

```python
tickers.login('my_email@gmail.com', 'my_password')
```

It will take around 15-20 seconds to log you in.  After that, utilize the following properties and methods to retrieve premium data:

```python
# Methods
tickers.p_balance_sheet()
tickers.p_income_statement()
tickers.p_cash_flow()

# The following allows you to retrieve premium reports and ideas related to a given symbol(s).  Report IDs and Idea IDs can be retrieved through the p_portal property
tickers.p_reports(report_id)
tickers.p_ideas(idea_id)

# Properties
tickers.p_company_360
tickers.p_portal
tickers.p_technical_events
tickers.p_value_analyzer
tickers.p_value_analyzer_drilldown
```

### Change Symbols

Instead of initializing another class with different symbols, simply do the following:

```python
tickers.symbols = 'goog amzn'
# or
tickers.symbols = ['goog', 'amzn']
```

## Fund Specific

Mutual Funds have many of the accessors detailed above as well as the additional ones below:

```python
fund = Ticker('rpbax')

fund.fund_category_holdings  # pandas.DataFrame
fund.fund_bond_ratings  # pandas.DataFrame
fund.fund_sector_weightings  # pandas.DataFrame
fund.fund_performance  # dict
fund.fund_bond_holdings  # dict
fund.fund_equity_holdings  # dict
```

## Options

Retrieve option pricing for every expiration date for given ticker(s)

```python
import pandas as pd
df = aapl.option_chain  # returns pandas.DataFrame

# The dataframe contains a MultiIndex
df.index.names
FrozenList(['symbol', 'expiration_date', 'option_type', 'row'])

# Get all options for specified symbol
df.loc['aapl']

# Get specific expiration date for specified symbol
df.loc['aapl', '2020-01-02']

# Get specific option type for expiration date for specified symbol
df.loc['aapl', '2020-01-02', 'calls']

# Works with multiple tickers as well
tickers = Ticker(['aapl', 'msft', 'fb'])
df = tickers.option_chain

# Retrieve options for only one symbol
df.loc['aapl']

# Retrieve only calls for all symbols
df.xs('calls', level=2)

# Retrieve only puts for fb
df.xs(('fb', 'puts'), level=[0, 2])
# or
df.xs(('fb', 'puts'), level=['symbol', 'option_type'])

# Filter dataframe by options that in the money
df.loc[df['inTheMoney'] == True]

# Only include Apple in the money options
df.loc[df['inTheMoney'] == True].xs('aapl')
```

## Historical Pricing

Historical price data can be retrieved for one or more tickers through the `history` method.

```python
aapl.history()
```

If no arguments are provided, as above, default values will be supplied for both `period` and `interval`, which are `ytd` and `1d`, respectively.  Additional arguments you can provide to the method are `start` and `end`.  Start and end dates can be either strings with a date format of `yyyy-mm-dd` or as a `datetime.datetime` object.

```python

aapl.history(period='max')
aapl.history(start='2019-05-01')  # Default end date is now
aapl.history(end='2018-12-31')  # Default start date is 1900-01-01

# Period options = 1d, 5d, 1mo, 3mo, 6mo, 1y, 2y, 5y, 10y, ytd, max
# Interval options = 1m, 2m, 5m, 15m, 30m, 60m, 90m, 1h, 1d, 5d, 1wk, 1mo, 3mo
```

Available periods and intervals can be seen through `Ticker.PERIODS` and `Ticker.INTERVALS`, respectively.

If trying to retrieve more than one ticker, one dataframe will be returned and the ticker can be accessed in the `symbol` level of the `pandas.MultiIndex`.

```python
tickers = Ticker('aapl msft')
tickers.history()
```
| symbol | date                |   volume |    open |    low |   high |   close |
|:-------|:--------------------|---------:|--------:|-------:|-------:|--------:|
| AAPL   | 2019-01-02 07:30:00 | 37039700 | 154.89  | 154.23 | 158.85 |  157.92 |
| AAPL   | 2019-01-03 07:30:00 | 91312200 | 143.98  | 142    | 145.72 |  142.19 |
| MSFT   | 2019-12-12 07:30:00 | 24612100 | 151.65  | 151.02 | 153.44 |  153.24 |
| MSFT   | 2019-12-13 14:00:01 | 23850062 | 153.003 | 152.85 | 154.89 |  154.53 |

## Multiple Endpoints

### New in 2.0.0

The property and method to retrieve multiple endpoints have changed:
- from `get_endpoints` to `get_modules`
- from `all_endpoints` to `all_modules`

### Accessing Multiple Modules

Multiple endpoints can be accessed in one call for a given symbol through two separate modules:  `get_modules` and `all_modules`.  The `get_modules` method
takes in a `list` or `str` of allowable modules.  Conversely, the `all_modules` property will retrieve all modules.

```python
aapl = Ticker('aapl')
modules = ['assetProfile', 'esgScores', 'incomeStatementHistory']
# or
modules = ['assetProfile esgScores incomeStatementHistory']
data = aapl.get_modules(modules)

# or

data = aapl.all_modules

# The symbol(s) and modules become the keys in the dictionary

data['aapl']['assetProfile']
data['aapl']['esgScores']
data['aapl']['incomeStatementHistory']
```

### Notes

- The data will always be returned as a dictionary
- `Ticker.MODULES` will show you the list of allowable modules you can pass to the `get_modules` method

## Miscellaneous Functions (New in 2.0.0)

Additional data can be obtained from Yahoo Finance outside of the `Ticker` class.  The following functions can be utilized to retrieve
additional data unrelated to a ticker symbol:

```python
from yahooquery import get_currencies, get_market_summary, get_trending
```

They take in keyword arguments of `lang`, `region`, and `corsDomain`.  The defaults are as follows:

```python
default = {
    'lang': 'en-US',
    'region': 'US',
    'corsDomain': 'finance.yahoo.com'
}
```

Those defaults, or keyword arguments, are used as query parameters in the requests made to Yahoo Finance.

```python
# Obtain a list of all currencies
d = get_currencies()

# View market summary statistics
d = get_market_summary()

# View trending tickers for a region (default is 'US')
d = get_trending()
```

One more function allows you to view a list of exchanges Yahoo Finance supports.  It takes no arguments or keyword arguments and returns a `pandas.DataFrame`.

```python
from yahooquery import get_exchanges

df = get_exchanges()
```
