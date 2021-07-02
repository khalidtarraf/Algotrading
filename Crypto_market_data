import requests                    # for "get" request to API
import json                        # parse json into a list
import pandas as pd                # working with data frames
import datetime as dt              # working with dates
import matplotlib.pyplot as plt    # plot data
import qgrid                       # display dataframe in noteboo
from datetime import timedelta
import numpy as np


def get_binance_bars(symbol, interval, startTime, endTime):
    url = "https://api.binance.com/api/v3/klines"

    startTime = str(int(startTime.timestamp() * 1000))
    endTime = str(int(endTime.timestamp() * 1000))
    limit = '1000'

    req_params = {"symbol": symbol, 'interval': interval, 'startTime': startTime, 'endTime': endTime, 'limit': limit}

    df = pd.DataFrame(json.loads(requests.get(url, params=req_params).text))

    if len(df.index) == 0:
        return None

    df = df.iloc[:, 0:6]
    df.columns = ['datetime', 'open', 'high', 'low', 'close', 'volume']

    df.open = df.open.astype("float")
    df.high = df.high.astype("float")
    df.low = df.low.astype("float")
    df.close = df.close.astype("float")
    df.volume = df.volume.astype("float")

    df['adj_close'] = df['close']

    df.index = [dt.datetime.fromtimestamp(x / 1000.0) for x in df.datetime]

    return df


datadir = 'C:\\Users\\khali\Desktop\\trading\\data\\'
start = dt.datetime(2020, 6, 26)
end = dt.datetime(2021, 6, 26)
symbol = "BTCUSDT"
interval = '1m'
mkt_data = pd.DataFrame()


while end >= start:
    dur_left = (end - start)
    dff = get_binance_bars(symbol, interval, start, end)
    print(start, "    Duration left:   ", dur_left)
    start = start + timedelta(hours=0, minutes=1000)
    mkt_data = mkt_data.append(dff)

print(mkt_data)
mkt_data.to_csv(datadir + "btc" + '.csv')
