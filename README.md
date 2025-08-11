# Bitcoin Price Data Ingestion with Streamz

A Python application for collecting real-time Bitcoin price data using cryptocurrency APIs and streaming data ingestion with Streamz. This tool provides continuous monitoring of Bitcoin prices with built-in rate limiting, error handling, and data visualization.

## Features

- **Real-time data collection** from cryptocurrency APIs
- **Dual API support**: CoinGecko (primary) and CryptoCompare (fallback)
- **Rate limiting protection** with exponential backoff
- **Live data visualization** with matplotlib
- **Streamz integration** for reactive data processing
- **CSV export** functionality for data persistence
- **Error handling** and retry mechanisms

## Installation

Install the required dependencies:

```bash
pip install streamz requests pandas matplotlib
```

## Quick Start

### Using CoinGecko API (Default)

```python
# Run the data collection for 60 seconds
collected_data = start_data_collection(60)

# Save data to CSV
bitcoin_data.to_csv('bitcoin_price_data.csv', index=False)
```

### Using CryptoCompare API (Alternative)

```python
# Switch to alternative API
setup_alternative_api()

# Run data collection for 5 minutes
collected_data = start_data_collection(300)
```

## API Configuration

### CoinGecko API (Primary)
- **Endpoint**: `https://api.coingecko.com/api/v3/simple/price`
- **Rate Limit**: Conservative approach with 30-second intervals
- **Data Includes**: Price, market cap, 24h volume, 24h change percentage
- **Free Tier**: No API key required

### CryptoCompare API (Fallback)
- **Endpoint**: `https://min-api.cryptocompare.com/data/price`
- **Rate Limit**: 5-second backoff intervals
- **Data Includes**: Price, market cap, 24h volume, 24h change percentage
- **Free Tier**: No API key required

## Data Structure

The collected data includes the following fields:

| Field | Description | Type |
|-------|-------------|------|
| `timestamp` | Data collection timestamp | DateTime |
| `price_usd` | Current Bitcoin price in USD | Float |
| `market_cap_usd` | Market capitalization in USD | Float |
| `24h_volume_usd` | 24-hour trading volume in USD | Float |
| `24h_change_percent` | 24-hour price change percentage | Float |

## Configuration Options

### Polling Interval
```python
POLL_INTERVAL_SECONDS = 30  # Time between API calls
```

### Collection Duration
```python
# Collect data for specified duration (in seconds)
start_data_collection(duration_seconds=300)  # 5 minutes
```

### Rate Limiting
```python
def fetch_bitcoin_data(max_retries=3, initial_backoff=10):
    # Built-in exponential backoff for rate limit handling
```

## Functions Reference

### Core Functions

#### `fetch_bitcoin_data(max_retries=3, initial_backoff=10)`
Fetches Bitcoin data from the CoinGecko API with retry logic and rate limiting protection.

**Parameters:**
- `max_retries`: Maximum number of retry attempts (default: 3)
- `initial_backoff`: Initial backoff time in seconds (default: 10)

**Returns:** Dictionary with Bitcoin market data or None if failed

#### `process_data(data)`
Processes incoming data, updates the global DataFrame, and displays real-time visualization.

**Parameters:**
- `data`: Dictionary containing Bitcoin market data

#### `start_data_collection(duration_seconds=300)`
Main function to run the data collection process for a specified duration.

**Parameters:**
- `duration_seconds`: Duration to collect data in seconds (default: 300)

**Returns:** DataFrame containing all collected data

### Alternative API Functions

#### `setup_alternative_api()`
Switches the data collection to use CryptoCompare API instead of CoinGecko.

#### `fetch_cryptocompare_data(max_retries=3, initial_backoff=5)`
Fetches Bitcoin data from CryptoCompare API with similar retry logic.

## Usage Examples

### Basic Data Collection
```python
# Collect data for 2 minutes with live plotting
bitcoin_data = start_data_collection(120)
print(f"Collected {len(bitcoin_data)} data points")
```

### Extended Collection with CSV Export
```python
# Collect data for 30 minutes
extended_data = start_data_collection(1800)

# Export to CSV with timestamp
filename = f"bitcoin_data_{datetime.now().strftime('%Y%m%d_%H%M%S')}.csv"
extended_data.to_csv(filename, index=False)
print(f"Data saved to {filename}")
```

### Using Alternative API
```python
# Switch to CryptoCompare if CoinGecko is unavailable
setup_alternative_api()
crypto_compare_data = start_data_collection(600)  # 10 minutes
```

## Error Handling

The application includes robust error handling for common scenarios:

- **Rate Limiting (HTTP 429)**: Automatic exponential backoff
- **Network Errors**: Retry mechanism with configurable attempts
- **API Downtime**: Graceful degradation with error messages
- **Data Processing Errors**: Exception catching with continued operation

## Rate Limiting Best Practices

To be respectful to API providers:

- **Default interval**: 30 seconds between requests
- **Random jitter**: ±10% variance to avoid synchronized requests
- **Exponential backoff**: Automatic handling of rate limit responses
- **User-Agent header**: Identifies requests as educational/research

## Data Visualization

The application provides real-time visualization features:

- **Live price chart**: Updates with each new data point
- **Current metrics display**: Shows latest price and 24h change
- **Data collection progress**: Tracks total points collected
- **Time series plotting**: Historical price trend visualization

## Output Files

### CSV Export Format
```csv
timestamp,price_usd,market_cap_usd,24h_volume_usd,24h_change_percent
2024-01-15 10:30:00,42500.50,834567890123.45,28765432109.87,2.34
```

## Troubleshooting

### Common Issues

**Rate Limiting Errors**
```
Solution: Increase POLL_INTERVAL_SECONDS or wait before retrying
```

**API Connection Issues**
```
Solution: Check internet connection and try alternative API
```

**Memory Usage**
```
Solution: Implement data chunking for extended collection periods
```

### Debug Mode
Enable verbose logging by modifying the error handling sections:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## Contributing

To extend this application:

1. **Add new APIs**: Follow the pattern in `setup_alternative_api()`
2. **Enhance visualization**: Modify the `process_data()` function
3. **Add data processing**: Extend the Streamz pipeline
4. **Implement persistence**: Add database storage options

## License

This project is for educational and research purposes. Please respect the terms of service of the cryptocurrency APIs used.

## Disclaimer

This tool is for educational purposes only. Cryptocurrency prices are volatile and this data should not be used for financial decision-making without proper analysis and risk assessment.
