---
title: "Stocks notification: Building multi-arch docker image"
datePublished: Sun Aug 11 2024 09:51:21 GMT+0000 (Coordinated Universal Time)
cuid: clzpdxj0o000g0alaa9fa629d
slug: stocks-notification-building-multi-arch-docker-image
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723369948505/ecc2f62d-143b-4eff-8439-86515e392ea4.jpeg
tags: docker-images, arm64, multiarch

---

Last year, around the same time, I added a feature in [market data notification](https://yaphc.com/stocks-notification-tradingview-webhook-to-telegram) to scrape the stocks fear greed index from [https://edition.cnn.com/markets/fear-and-greed](https://edition.cnn.com/markets/fear-and-greed), with selenium and chrome webdriver.

## **Difference in development and production**

There is an issue: my development environment is on AMD64, while my production environment is on ARM64.

I switched from AWS T3 to T4g due to the superior price performance that the AWS graviton provides.

Previously, I used t3.micro, which has 2 vCPUs and 1GB of memory. However, 1GB was not enough because running Chrome and scraping the fear and greed data easily used up more than half of it, maxing out the available memory.

So, I searched and decided on t4g.small, which has 2 vCPUs and 2GB of memory.

## Changes in workflow

**1\. Multi-arch docker image for chrome**

Chrome is packaged for Ubuntu AMD64, not for ARM. Chromium is used for ARM instead.

A build argument [$TARGETPLATFORM](https://docs.docker.com/build/building/multi-platform/) is available for this purpose.

```dockerfile
RUN if [ "$TARGETPLATFORM" = "linux/amd64" ]; then \
    wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list && \
    apt update -y && apt install -y google-chrome-stable; \
elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then \
    wget https://launchpad.net/ubuntu/+archive/primary/+sourcefiles/chromium-browser/1:85.0.4183.83-0ubuntu2.22.04.1/chromium-browser_85.0.4183.83-0ubuntu2.22.04.1.tar.xz && \
    tar -xvf chromium-browser_85.0.4183.83-0ubuntu2.22.04.1.tar.xz && \
    cp chromium-browser-85.0.4183.83/chromedriver /usr/local/bin && \
    cp chromium-browser-85.0.4183.83/chromium-browser /usr/local/bin; \
fi
```

**2\. Build docker image locally instead of using github runner**

At that time, the standard practice to build multi-arch image is to use [qemu action](https://docs.docker.com/build/ci/github-actions/multi-platform/) to translate the instructions to the target platform.

However, the image took more than 40 minutes to build, making this approach impractical.

So, I build the image locally, push it to github container registry, then manually trigger the deploy action in github.

This break in the deployment pipeline is a small price to pay for a much faster feedback loop. I have not kept up to date on whether a better solution is available now.

```bash
docker buildx create --platform linux/amd64,linux/arm64 --name amd64_arm64 --node amd64_arm640 --use

docker buildx build --progress=plain \
  --platform=linux/amd64,linux/arm64 \
  --target=release \
  --cache-from=type=registry,ref=ghcr.io/hanchiang/market_data_notification:buildcache \
  --cache-to=type=registry,ref=ghcr.io/hanchiang/market_data_notification:buildcache,mode=max \
  --tag=ghcr.io/hanchiang/market_data_notification:"$GITHUB_COMMIT_SHA" \
  --push .
```

3. **Resource management for API server and chrome**
    

I have also set resource limits for the API server and Chrome to prevent similar out-of-memory issues in the future.

```bash
docker run --name $APP_NAME -p 8080:8080 --network host --memory="512m" --cpus="0.5"
# peak memory usage is around 600m
docker run --name chrome -p 4444:4444 -p 5900:5900 -p 7900:7900 --network host --memory="1g" --cpus="1" -d seleniarm/standalone-chromium:114.0-20230615
```

## Summary

To save some costs, I used an ARM64 processor in the production environment. This caused issues with package compatibility and longer image build times. However, the trade-off for this workaround is acceptable.

## Reference

Market data notification post: [https://yaphc.com/stocks-notification-tradingview-webhook-to-telegram](https://yaphc.com/stocks-notification-tradingview-webhook-to-telegram)

market data notification github: [https://github.com/hanchiang/market-data-notification](https://github.com/hanchiang/market-data-notification)