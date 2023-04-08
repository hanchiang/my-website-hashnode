---
title: "Stocks notification: TradingView webhook to telegram"
datePublished: Fri Apr 07 2023 12:00:39 GMT+0000 (Coordinated Universal Time)
cuid: clg6hwp10049quonvhh0da80m
slug: stocks-notification-tradingview-webhook-to-telegram
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1680860732766/aea33ad6-248c-47d4-9d65-45b1cdc403f6.jpeg
tags: python, side-project, tradingview, telegram-bot

---

At the start of the year, I worked on a telegram bot that sends market data of certain stocks for the previous day's close to a telegram channel.

**Features**

* Basic info such as closing price, EMA20, difference between closing price and EMA20
    
* Overextension from EMA20 based on the median delta when stock reverse in the next few days
    

**Example**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680859314366/2dc91eb9-d554-4d6f-9288-ba45382abe12.png align="center")

## Overview of the workflow

1. Create an indicator script in TradingView
    
2. Add indicator to chart
    
3. Set an alert using the indicator as condition, and configure webhook.
    
4. Once an alert is fired, the server saves the data in redis, and sends a notification to telegram before the market opens.
    

**What's not covered**

Infrastructure(set up server, HTTPS, etc), CICD, setting up an API server

Other data sources: VIX central

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680443428026/87e0668b-64ad-4a63-aab1-6a5668f79bb2.png align="center")

## Create a script in TradingView

### PineScript introduction

TradingView has its own programming language called [PineScript](https://www.tradingview.com/pine-script-docs/en/v5/Introduction.html), which allows us to create indicators and trading strategies.

Here are some recommended readings on PineScript:

[Execution model](https://www.tradingview.com/pine-script-docs/en/v5/language/Execution_model.html) explains how the script is executed on the chart, particularly the difference between historical and real-time bars.

[Types](https://www.tradingview.com/pine-script-docs/en/v5/language/Type_system.html) and [variable declaration](https://www.tradingview.com/pine-script-docs/en/v5/language/Variable_declarations.html).

[Alerts](https://www.tradingview.com/pine-script-docs/en/v5/concepts/Alerts.html) to send data to a self-defined destination.

### Create the script

[The script](https://www.tradingview.com/script/yKEUsKOl-Stock-data-alert-webhook-to-custom-API-server/) that I have created retrieves the closing price and the 1D Exponential Moving Average(EMA) 20 for some tickers.

It uses built-in functions [ta.ema](https://www.tradingview.com/pine-script-reference/v5/#fun_ta%7Bdot%7Dema) to get the EMA and [request.security](https://www.tradingview.com/pine-script-reference/v5/#fun_request%7Bdot%7Dsecurity) to get data for a particular ticker and timeframe, and `alert.freq_once_per_bar_close` to send the alert when the market closes.

**Ticker name**

The ticker argument for `request.security` needs to be in the format `<exchange><ticker>`, which can be gotten from the address bar after searching for the ticker in [tradingview.com](http://tradingview.com).

For SPY, it is [https://www.tradingview.com/symbols/AMEX-SPY/](https://www.tradingview.com/symbols/AMEX-SPY/)

**Secret**

There is a self-defined secret `<your secret here>`, which is used to authenticate the request in the server.

```plaintext
//@version=5
indicator("Market data notification")

ema20 = ta.ema(close, 20)

array_start = '['
array_end = ']'
json_start = '{'
json_end = '}'

build_json_array(res, symbol, timeframe, closee, ema20) =>
    res + json_start + '"symbol": ' + symbol + ', "timeframe": ' + timeframe + ', "close": ' + closee + ', "ema20": ' + ema20 + json_end + ','


string[] json_key_values = array.new_string(0)

array.push(json_key_values, '"secret": "<your secret here>"')

data_payload = '"data": ' + array_start
data_payload := build_json_array(data_payload, '"SPY"', '"1d"', str.tostring(request.security("AMEX:SPY", "1D", close)), str.tostring(request.security("AMEX:SPY", "1D", ema20)))
data_payload := build_json_array(data_payload, '"QQQ"', '"1d"', str.tostring(request.security("NASDAQ:QQQ", "1D", close)), str.tostring(request.security("NASDAQ:QQQ", "1D", ema20)))
data_payload := build_json_array(data_payload, '"DJIA"', '"1d"', str.tostring(request.security("AMEX:DJIA", "1D", close)), str.tostring(request.security("AMEX:DJIA", "1D", ema20)))
data_payload := build_json_array(data_payload, '"IWM"', '"1d"', str.tostring(request.security("AMEX:IWM", "1D", close)), str.tostring(request.security("AMEX:IWM", "1D", ema20)))
data_payload := build_json_array(data_payload, '"AAPL"', '"1d"', str.tostring(request.security("NASDAQ:AAPL", "1D", close)), str.tostring(request.security("NASDAQ:AAPL", "1D", ema20)))
data_payload := build_json_array(data_payload, '"AMD"', '"1d"', str.tostring(request.security("NASDAQ:AMD", "1D", close)), str.tostring(request.security("NASDAQ:AMD", "1D", ema20)))
data_payload := build_json_array(data_payload, '"AMZN"', '"1d"', str.tostring(request.security("NASDAQ:AMZN", "1D", close)), str.tostring(request.security("NASDAQ:AMZN", "1D", ema20)))
data_payload := build_json_array(data_payload, '"BABA"', '"1d"', str.tostring(request.security("NYSE:BABA", "1D", close)), str.tostring(request.security("NYSE:BABA", "1D", ema20)))
data_payload := build_json_array(data_payload, '"COIN"', '"1d"', str.tostring(request.security("NASDAQ:COIN", "1D", close)), str.tostring(request.security("NASDAQ:COIN", "1D", ema20)))
data_payload := build_json_array(data_payload, '"GOOGL"', '"1d"', str.tostring(request.security("NASDAQ:GOOGL", "1D", close)), str.tostring(request.security("NASDAQ:GOOGL", "1D", ema20)))
data_payload := build_json_array(data_payload, '"META"', '"1d"', str.tostring(request.security("NASDAQ:META", "1D", close)), str.tostring(request.security("NASDAQ:META", "1D", ema20)))
data_payload := build_json_array(data_payload, '"MSFT"', '"1d"', str.tostring(request.security("NASDAQ:MSFT", "1D", close)), str.tostring(request.security("NASDAQ:MSFT", "1D", ema20)))
data_payload := build_json_array(data_payload, '"NFLX"', '"1d"', str.tostring(request.security("NASDAQ:NFLX", "1D", close)), str.tostring(request.security("NASDAQ:NFLX", "1D", ema20)))
data_payload := build_json_array(data_payload, '"NVDA"', '"1d"', str.tostring(request.security("NASDAQ:NVDA", "1D", close)), str.tostring(request.security("NASDAQ:NVDA", "1D", ema20)))
data_payload := build_json_array(data_payload, '"TSLA"', '"1d"', str.tostring(request.security("NASDAQ:TSLA", "1D", close)), str.tostring(request.security("NASDAQ:TSLA", "1D", ema20)))
data_payload := build_json_array(data_payload, '"VIX"', '"1d"', str.tostring(request.security("TVC:VIX", "1D", close)), str.tostring(request.security("TVC:VIX", "1D", ema20)))

// remove trailing comma from array
data_payload := str.substring(data_payload, 0, str.length(data_payload) - 1)
data_payload := data_payload + array_end

array.push(json_key_values, data_payload)

array.push(json_key_values, '"test_mode": "false"')

result_json = json_start + array.join(json_key_values, ',') + json_end

alert(result_json, alert.freq_once_per_bar_close)
```

**Add the script in tradingview**

Open the **Pine Editor** tab, add the script, and add it to the chart.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680447035453/4f75d953-a214-439d-9cba-c0b70a4c3296.png align="center")

**Set the alert and webhook**

Set the alert using the indicator in the previous step as the condition, and configure webhook to send it to your server.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680447164094/db0dfb2f-3bb5-48b8-8a57-57c46a39e894.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680447170876/aa3393e8-cc37-47a6-9a57-56b49e3b44a2.png align="center")

Note: Make sure the timeframe of the chart is set to daily(1D), so that the script will get triggered only once a day, when the market closes.

## Data storage: Redis

Redis is chosen because it fits my need for a key-value storage, and has the option to [persist data](https://redis.io/docs/management/persistence/).

#### Data persistence

Redis offers 2 options: Redis Database(RDB) and Append Only File(AOF).

RDB is a [snapshot](https://github.com/redis/redis/blob/7.0/redis.conf#L414-L433) of the data at specific intervals, while AOF is an [append-only log](https://github.com/redis/redis/blob/7.0/redis.conf#L1361-L1377) of the write operations received by the server.

The tradeoffs of RDB and AOF complement each other, so it makes sense to use [both](https://github.com/hanchiang/market-data-notification-infra/blob/master/images/scripts/install-redis.sh#L22-L25).

#### Data structure

[Sorted set](https://redis.io/docs/data-types/sorted-sets/) is chosen to store the data because the elements are sorted by score, which is the timestamp, and supports finding elements by score quickly.

## Receive data from tradingview and post to telegram

We need a regular API server that can receive POST request from tradingview, and send it to telegram. I wrote it in Python, using the [FastAPI](https://fastapi.tiangolo.com/) framework, because it is just fast to develop with.

### Webhook authenticity check

Since the webhook is a HTTP POST request that anyone can call, we need to ensure that it actually originates from tradingview, not someone else.

The first safeguard is to check the self-defined **secret** in the PineScript.

The second is to check if the request comes from one of [tradingview's machines](https://www.tradingview.com/support/solutions/43000529348-about-webhooks/).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680448851118/3b274f6e-e8aa-45a6-b1bc-51cdf4895493.png align="center")

### Create telegram bot

#### Set up bot and channel

Follow [this guide](https://manual.activechat.ai/fundamentals/messaging-channels/telegram) to create a telegram bot, and note down the token, which is used to interact with the [telegram bot API](https://core.telegram.org/bots/api).

Add the bot as an **administrator** to the channel so that it can send messages.

**Send a message to the channel**

The **channel id** of the channel is needed for the bot to send the message.

There are two ways of getting the channel id:

First, go to the channel on telegram web and look at the URL, which is something like `https://web.telegram.org/k/#-YYYYYYYYY`. The channel id will be `-100YYYYYYYYYY`.

The second method is to use [a bot](https://t.me/username_to_id_bot) to tell you the channel id.

#### Use a library to send the message

There are many [client libraries](https://core.telegram.org/bots/samples) for different languages. I chose [python-telegram-bot](https://python-telegram-bot.org/).

### Cron job

Finally, the last piece of the puzzle is to schedule the message 30 minutes before the market opens(9.30AM local time) using a cron job.

This is the [script](https://github.com/hanchiang/market-data-notification/blob/master/src/job/stocks.py) for sending the message.

#### Timezone

One thing to note is that the timezone changes due to daylight savings time(DST). Without DST, the timezone is Pacific Standard Time(PST), which is **UTC-5**. With DST, it is Pacific Daylight Time(PDT) which is **UTC-4**.

On the application side, we just need to check against the local time:

```python
def should_run() -> bool:
    if config.get_is_testing_telegram():
        return True

    now = get_current_datetime()
    local = get_current_datetime()
    local = local.replace(hour=config.get_stocks_job_start_local_hour(), minute=config.get_stocks_job_start_local_minute())
    delta = now - local

    should_run = abs(delta.total_seconds()) <= config.get_job_delay_tolerance_second()
    print(
        f'local time: {local}, current time: {now}, local hour to run: {config.get_stocks_job_start_local_hour()}, local minute to run: {config.get_stocks_job_start_local_minute()}, current hour {now.hour}, current minute: {now.minute}, delta second: {delta.total_seconds()}, should run: {should_run}')
    return should_run
```

On the cron side, the script needs to run on the hour with and without DST.

This is the cron used to run the script at 9AM every day from monday to saturday:

```bash
# In UTC
0 13,14 * * 1-6
```

## Local development

Since a webhook requires a public HTTPS endpoint, we need a reverse proxy to tunnel traffic from the public to our local server.

[Ngrok](https://ngrok.com/) is simple and easy to use.

## Summary

This is an overview of how to use push data from Tradingview to our own server and send it to telegram.

Tradingview supports more complex workflows such as executing trades based on certain strategies.

## Reference

**Telegram**

My channel: [https://t.me/+6RjlDOi8OyxkOGU1](https://t.me/+6RjlDOi8OyxkOGU1)

Telegram bot API: [https://core.telegram.org/bots/api](https://core.telegram.org/bots/api)

Python telegram bot: [https://docs.python-telegram-bot.org/en/stable/](https://docs.python-telegram-bot.org/en/stable/)

**Code**

Github link for backend: [https://github.com/hanchiang/market-data-notification](https://github.com/hanchiang/market-data-notification)

Github link for infra setup and app automation: [https://github.com/hanchiang/market-data-notification-infra](https://github.com/hanchiang/market-data-notification-infra)

API server: [https://github.com/hanchiang/market-data-notification/blob/master/src/server.py](https://github.com/hanchiang/market-data-notification/blob/master/src/server.py)

Script for sending message: [https://github.com/hanchiang/market-data-notification/blob/master/src/job/stocks.py](https://github.com/hanchiang/market-data-notification/blob/master/src/job/stocks.py)

**Redis**

Persistence: [https://redis.io/docs/management/persistence/](https://redis.io/docs/management/persistence/)

My persistence config: [https://github.com/hanchiang/market-data-notification-infra/blob/master/images/scripts/install-redis.sh#L22-L25](https://github.com/hanchiang/market-data-notification-infra/blob/master/images/scripts/install-redis.sh#L22-L25)

**TradingView PineScript**

PineScript doc: [https://www.tradingview.com/pine-script-docs/en/v5/index.html](https://www.tradingview.com/pine-script-docs/en/v5/index.html)

PineScript reference: [https://www.tradingview.com/pine-script-reference/v5/](https://www.tradingview.com/pine-script-reference/v5/)

My PineScript: [https://www.tradingview.com/script/yKEUsKOl-Stock-data-alert-webhook-to-custom-API-server/](https://www.tradingview.com/script/yKEUsKOl-Stock-data-alert-webhook-to-custom-API-server/)