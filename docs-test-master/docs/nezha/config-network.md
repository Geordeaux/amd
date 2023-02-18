---
id: config-network
title: Network
sidebar_label: Network
---

## Network

When using web-related APIs in Mini Programs, developers should note the following issues in advance.

### 1. Configuration of Server Domain Name

Each Mini Program needs to be set with a communication domain name in advance. The Mini Program **can only communicate with the specified domain name over the network**. This includes normal HTTPS [request](https://nezha.fe.devfdg.net/docs/rpc-action#request), [download-file](https://nezha.fe.devfdg.net/docs/rpc-action#download-file).

#### Configuration process

Configure the server domain name in **Mini Program Console** > **Settings** > **Development Settings** > **Server Domain Name**. Configuration notes:

- Domain names only support `https`  protocols.
- An IP address (except for the Mini Program's IP address in the [LAN](https://developers.weixin.qq.com/miniprogram/dev/framework/ability/mDNS.html)) or localhost cannot be used for a domain name.
- You can configure a port, such as https://myserver.com:8080. However, after configuration, requests can only be sent to https://myserver.com:8080, and those sent to https://myserver.com, https://myserver.com:9091 or another URL will fail.
- If you do not configure a port, such as https://myserver.com, the request URL cannot include a port, not even the default port 443; otherwise, requests sent to https://myserver.com:443 will fail.
- **For security reasons, `api.binance.com` cannot be configured as a server domain name, and related APIs cannot be called in Mini Programs.** Developers should save the AppSecret to the backend server, and use the `getAccessToken` API to get `access_token` and call the relevant APIs via the server.

### 2. Network Request

#### Use Limits

- The `referer` header of a network request is optional. Its format is always `https://www.binance.com`. 
- The maximum concurrency of [request](https://nezha.fe.devfdg.net/docs/rpc-action#request), [download-file](https://nezha.fe.devfdg.net/docs/rpc-action#download-file) is **10**.
- When the Mini Program is running in the background, if a network request does not end within **5s**, an error message of `fail interrupted` will be called back. The network request API cannot be called until the Mini Program client is re-opened.

#### Return value encoding

- The values returned by the server should be **UTF-8** encoded. The Mini Program will try to convert non-UTF-8 encoded return values but may fail.
- The Mini Program automatically filters the BOM header (only one BOM header is filtered).

#### Callback function

- **As long the value returned by the server is received, the `success` callback is triggered regardless of `statusCode`. Determine the return value based on the business logic.**

### 3. FAQ

#### Skip domain name verification

The server domain name will not be verified when debugging mode is enabled on your phone.

**After the server domain name is configured, we recommend you disable this option during development and test the domain name on each platform to ensure it is correct.**

#### Domain acceleration

When the first-party Mini Program requests the binance private API, please use **https://www.binance.com** as the domain name, and the native client will switch to the most suitable domain according to the domain acceleration policy.

