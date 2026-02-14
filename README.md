# finnomena-api-unofficial

This is an unofficial API for finnomena.com - a Thai financial platform for investing in mutual funds.

## ⚠️ Important Changes (v2)

This is an updated version that works with Finnomena's new API endpoints. The original API endpoints have been deprecated.

### Changes from v1:
- ✅ Fixed `get_fund_info()` - Now uses `/fn3/api/fund/public/{id}` endpoint
- ✅ Fixed `get_fund_price()` - Now uses [pythainav](https://github.com/ninyawee/pythainav) library with Finnomena v2 API
- 🔒 Added `LOCK_UNTESTED` configuration for security
- ⚠️ Login-related features are locked by default (untested)

## Installation

```bash
pip install -r requirements.txt
```

## Quick Start

```python
from finnomena_api.api import finnomenaAPI

api = finnomenaAPI()

# Get list of all funds
funds = api.get_fund_list()
print(f"Total funds: {len(funds)}")

# Get fund information
info = api.get_fund_info("DAOL-GOLD")
print(f"Fund: {info['security_name']}")
print(f"Day change: {info['d_change']}%")

# Get historical prices
prices = api.get_fund_price("DAOL-GOLD", "1M")
print(prices)
```

## Available Methods

### Public Methods (No login required)
- ✅ `get_fund_list()` - Get list of all available funds
- ✅ `get_fund_info(sec_name)` - Get fund information (fees, day change, etc.)
- ✅ `get_fund_price(sec_name, time_range)` - Get historical NAV prices

### Login Required Methods (Locked by default)
- 🔒 `login()` - Login to Finnomena account
- 🔒 `get_account_status()` - Get account status
- 🔒 `get_port_status(port_name)` - Get portfolio status
- 🔒 `get_order_history(port_name)` - Get order history

## Configuration

### LOCK_UNTESTED

For security, login-related methods are **locked by default** since they haven't been tested.

To unlock:
```bash
export LOCK_UNTESTED=true
python your_script.py
```

| Value | Status |
|-------|--------|
| `true` | 🔓 Unlocked - All methods available |
| `false` or not set | 🔒 Locked - Only public methods available |

### Error when locked:
```python
api.login()
# RuntimeError: Method 'login' is locked because it hasn't been tested. 
# Set LOCK_UNTESTED=true to unlock.
```

## get_fund_price() Time Ranges

Available time ranges: `1D`, `7D`, `1M`, `3M`, `6M`, `1Y`, `3Y`, `5Y`, `10Y`, `MAX`

```python
# Get 1 month of price data
prices = api.get_fund_price("DAOL-GOLD", "1M")

# Get all available historical data
prices = api.get_fund_price("DAOL-GOLD", "MAX")
```

## Dependencies

- pandas
- requests
- BeautifulSoup4
- pythainav (new)

## Data Sources

- **Fund Info**: `https://www.finnomena.com/fn3/api/fund/public/{mstar_id}`
- **Fund Prices**: `https://www.finnomena.com/fn3/api/fund/v2/public/funds/{mstar_id}/nav/q` (via pythainav)

## License

MIT License

## Disclaimer

This is an unofficial API. Use at your own risk. Not affiliated with Finnomena.
