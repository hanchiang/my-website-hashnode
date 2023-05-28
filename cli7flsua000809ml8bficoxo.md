---
title: "Reverse engineering CryptoPanic REST API"
datePublished: Sun May 28 2023 13:03:22 GMT+0000 (Coordinated Universal Time)
cuid: cli7flsua000809ml8bficoxo
slug: reverse-engineering-cryptopanic-rest-api
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685276176582/fec37267-b5e1-46b2-8cbe-7f523767e7cd.png
tags: aes, reverse-engineering, symmetric-encryption, cryptopanic

---

[CryptoPanic](https://cryptopanic.com/) is a news aggregator website for trending news based on social sentiment. It's a good website for keeping up to date on the latest news, and its compact layout reminds me of hacker news.

I was looking through the network requests and got intrigued after finding out that the main data is not in plaintext:

![Posts request encrypted data](https://cdn.hashnode.com/res/hashnode/image/upload/v1684933900016/21180fe6-5d97-48ae-abbf-a18237c27228.png align="center")

The server sends some sort of encoded data, and the client decodes it. After digging around the js bundle, I manage to recover the plaintext data that is displayed on the website.

*Note: This post is for educational purposes only.*

## The reverse engineering process

### 1\. Open the js bundle

The first thing is to find out which part of the javascript code is responsible for decoding this response. Begin by searching for and opening the js bundle `cryptopanic.min.xxxxx.js`.

![Chrome consoel js bundle](https://cdn.hashnode.com/res/hashnode/image/upload/v1684934522582/8edfd255-6ccc-4827-a3e8-2c755efdf099.png align="center")

### 2\. Locate the part of the code that decodes the data

Searching for "decrypt" immediately zooms in on this function called `dc` that does the core work of decoding the data.

`dk()` returns the encryption key, together with parameter `t` which is used as the Initialization Vector(IV), are passed through the AES algorithm with zero padding.

`wordArrayToByteArray` converts the decrypted response into a byte array, and `pako.default.inflate` decompresses the data, and converts them into javascript's utf-16.

![Decrypt and inflate](https://cdn.hashnode.com/res/hashnode/image/upload/v1684934721630/84f85625-e91b-4c3a-832b-69c8c26a8f5e.png align="center")

After this step, the result is a JSON string of the API response. The JSON version looks like this:

![Posts raw data](https://cdn.hashnode.com/res/hashnode/image/upload/v1685276053358/a39775b8-ba8a-41c6-a7e4-5cab0f6c7cd5.png align="center")

### 3\. Transform the raw response into a usable array of objects

The last step is to transform the raw JSON into an array of objects so that it is easy to work with.

![Posts normalized data](https://cdn.hashnode.com/res/hashnode/image/upload/v1685275897099/2e8180ed-7701-4f68-8782-03f1b0ccd3fe.png align="center")

Revisiting the arguments passed to `dc` earlier, the first argument `t`(IV) is the first 16 characters of the module and some string or the CSRF token.

For posts API, `t` is `news` and `n` is empty.

![Decoding posts caller](https://cdn.hashnode.com/res/hashnode/image/upload/v1685276708104/c94a9a2e-9b89-4dc0-b619-6e7da41331ce.png align="center")

For dashboard API, `t` is also `news` while `n` is `rnlistrnlistrnlistrnlist`.

![Decoding dashboard caller](https://cdn.hashnode.com/res/hashnode/image/upload/v1685276913945/ebedbcc5-304a-4737-8005-530b55508413.png align="center")

### 4\. The final output

This is the final output of the posts and dashboard response after decrypting, decompressing and normalizing.

![Posts normalized](https://cdn.hashnode.com/res/hashnode/image/upload/v1685277081971/a75e011f-9512-4391-b940-ea341b13242b.png align="center")

![Dashboard normalized](https://cdn.hashnode.com/res/hashnode/image/upload/v1685277094868/a54111f0-113a-4f0e-bbf7-14f87b21e85f.png align="center")

![Cryptopanic home page highlighted](https://cdn.hashnode.com/res/hashnode/image/upload/v1685277172266/68b886e0-959c-4b9b-9743-350803a4847c.png align="center")

## Try it out yourself

Here is a [gist](https://gist.github.com/hanchiang/ae411d08cb238ccefa0aef4b2c2899e0) that you can use to try decoding the response yourself.

**How to use**

1. Create an `.env` file with `CRYPTOPANIC_ENCRYPTION_KEY` and `CRYPTOPANIC_CSRF_TOKEN`.
    
2. Install the dependencies: `npm install dotenv pako crypto-js`
    
3. Run `node decrypt.js [post|dashboard]`
    

%[https://gist.github.com/hanchiang/ae411d08cb238ccefa0aef4b2c2899e0]