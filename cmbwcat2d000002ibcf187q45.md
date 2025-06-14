---
title: "Reverse engineering CoinMarketCap API"
datePublished: Sat Jun 14 2025 14:36:33 GMT+0000 (Coordinated Universal Time)
cuid: cmbwcat2d000002ibcf187q45
slug: reverse-engineering-coinmarketcap-api
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1749909170755/49f31678-2c27-49da-a7c5-b501b706228c.png
tags: apis, reverse-engineering, coinmarketcap

---

Earlier, I [discussed](https://yaphc.com/stocks-notification-tradingview-webhook-to-telegram) how to set up a TradingView webhook and send notifications to telegram for stocks.

The full workflow looks something like this:

* Before the market opens, send a telegram notification with stock data from TradingView, VIX Central, and CNN fear and greed index.
    
* When the market closes, receive the TradingView webhook and save the data in Redis.
    

## Crypto telegram notification workflow

There is a similar workflow for crypto as well. Before the market opens, a telegram notification is sent with data from CoinMarketCap and the [alternative.me](https://alternative.me/crypto/fear-and-greed-index/) fear and greed index. However, there is no TradingView webhook to receive data from.

## Locating the data

*Note: This post is for educational purpose only.*

This is the [page](https://coinmarketcap.com/cryptocurrency-category/) on CoinMarketCap where I found the data for the 24-hour price change by sectors.

  
This page is server-side rendered (SSR), so there is no API request for this data when you inspect the network tab.

CoinMarketCap uses NextJS, which stores the SSR data in `window.__NEXT_DATA__`, and the data we are interested in is located at `props.pageProps.data`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749476854115/ed00ab39-dd3c-4b3f-b1d7-db6482567441.png align="center")

You can also find it in the page source.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749909694394/27f63029-51f8-4cd1-8220-e2f83eea275c.png align="center")

## Finding the API request

Getting the data from HTML is okay, but having an API is better.

In Chrome's source tab, locate the Next.js bundle under the `s2.coinmarketcap.com/cmc/_next/static/chunks/pages` folder. There, you will find `_app**.js` and `cryptocurrency-category-**.js`.

`_app**.js` contains the application-level code that is bundled for every page.

`cryptocurrency-category-**.js` contains the code specific to this page.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749478536712/d685b11a-8ab6-4fac-b571-f1e978173543.png align="center")

### Older version of cryptocurrency-category\*\*.js

I still have an older copy of the JavaScript file for this page, which was retrieved sometime last year. The API call is found within a call to `F()`.

`getInitialProps` is a NextJS lifecycle method that runs when the page is loaded.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749480427048/478bc2b0-034b-460a-944f-36a64d9c0fd6.png align="center")

Clicking into the function reveals that the endpoint is at `/data-api/v3/sector/w/list`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749480624975/f4982430-62d3-45ff-8ff4-86f5196c3b60.png align="center")

Requesting data from the endpoint gives the outdated version of the top sectors by 24-hour price change, which doesn't match what is shown on the page.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749480886107/6957c379-ea69-47dc-b210-e7498002a226.png align="center")

### Current version of cryptocurrency-category\*\*.js

With the updated version of `cryptocurrency-category**.js`, the API call is at `L()`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749481401975/e8a4a17e-d9d7-4bc3-a127-b305a762262c.png align="center")

Clicking into the function reveals that the endpoint is `/data-api/v4/sector/m/full-list`.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749481447099/21be38b2-46f5-4b8e-b3e9-842aa7755ab6.png align="center")

Requesting data from the endpoint provides the current list of top sectors by 24-hour price change, which is displayed on the page.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749481556728/12e7308d-3c63-437c-bf23-27c9585d07bf.png align="center")

### API domain

The above 2 requests referenced the `javaApi` endpoint, which can be found in `_app**.js`

## Tips

**NextJS knowledge**

Since the page is written in NextJS, it's helpful to know where NextJS stores client-side data and understand the lifecycle method for fetching data.

**Trial and error**  
Use the good old debugger to set a breakpoint, reload the page, and check if it gets triggered. If it does, there's a good chance it's the API request we're looking for.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749911390353/2ffe09b2-87a8-47bd-acc7-025a06f3cba6.png align="center")