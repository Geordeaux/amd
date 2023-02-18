---
id: arch-prefetch
title: Prefetch Strategy
---
> Timing：Binance App Cold Start

Binance App cold started, requests the prefetch api. The MP resource that is needed would be downloaded concurrently (reuse the code of [Dynamic update strategy](https://docs.fe.devfdg.net/docs/nezha/arch-download)) if the response is not empty. If a user opens a MP that is prefetching, the loading page shows, and then starts the MP when prefetch finishes.

![Prefetch](https://static.devfdg.net/static/mono-static/docs-ui/img/arch/arch-prefetch.png)
