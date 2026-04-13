# polymarket-BTC5min-database
A collection of orderbook snapshots from polymarket BTC 5 min markets to be used to test your bots and strategies :)

## License
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

This work is licensed under a [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).

### Disclaimer
This data is for educational and testing purposes only. Crypto markets involve risk. The author is not responsible for any financial losses incurred from the use of this data.

## Format
**Asset:** Bitcoin (BTC) Prediction Markets 
**Frequency:** 5-minute snapshots 
**Format:** Apache Parquet (highly compressed, fast to load in Python/R) 
**Content:** Unix timestamp, slug, asset id, top 100 bids, 100 asks, raw orderbook
**File Name:** slug_time-of-the-save (unix time)

Check the *How to parse* section for more info.
For the orderbook structure and use, check out the official Polymarket documentation:
[Price and Orderbook](https://docs.polymarket.com/concepts/prices-orderbook)
[Receving Orderbook from web socket](https://docs.polymarket.com/market-data/websocket/market-channel)

## How the data is captured
I subscribe to the BTC5min market via [websocket](https://docs.polymarket.com/market-data/websocket/overview) and save every change to the order book; every 300 updates, I create a parquet file with the data received. 
I automatically switch to the current 5min market: therefore, the data of the given market started from when that market begins.

## How to parse
### Python
You would need pandas, pyarrow.

    import  pyarrow.parquet  as  pq
    import  pandas  as  pd
    
    # Reading the files from folder
    dataset = pq.read_table('FOLDER-WITH-THE-PARQUET-FILES')
    # Create pandas dataframe
    df  =  dataset.to_pandas()
    # If you want, convert timestamp to datatime object
    df['timestamp'] =  pd.to_datetime(df['timestamp'], unit='s')
    # Check if everything works
    df.head(5)

To access the data, you can load it using json module:

    import json
    # Example of reading the first book
    first_book = json.load(df.loc[0,'raw_bdata'])
    first_book
    
    # Get the bids
	first_book.get('bids')

### Schema

    timestamp: double 
    slug: large_string 
    asset_id: large_string 
    bids: large_string 
    asks: large_string 
    raw_data: large_string

    
## Suggestions
If you got any suggestion or requests, please let me know :)


## Support this Project
If you find this data useful for your trading bots, consider supporting the maintenance of this dataset:

* **BTC:** `3N2g1KRu4L87g3sYAn8EbPxF9NSX9vDtJa`
* **USDC:** `comin'

