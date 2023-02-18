---
id: arch-management
title: management
sidebar_label: management
---

## Overview

For third party, management is all the works besides code development, including **upload code**, **manage version**, and **delivery content to user**,  etc.

![mp-management](https://static.devfdg.net/static/mono-static/docs-ui/img/mp-management.png)

### Accounts ↔︎ Mini-program ↔︎ Version ↔︎ Statues

- Accounts is based on the binance account plus the permission
- One account can own multiple Mini-program
- One Mini-program can own multiple version
- One Version can in different status in different situation
  - development
  - trail
  - audit
  - online
  - suspend

## Basic set up

- Lowest compatible version

  If user use outdated version of app, need to be notified to update if he/she want to use the mini program.

- Api_whilteList

  Use thrid party api such as http://companyA/api

- Permission List

  Permission details

## Reference

[API](/docs/nezha/api-mp-overview)

## upload tutorial

1. login the website

    * url: https://developers.devfdg.net/en/mpp
    * account
        * username: testtonyahall@yahoo.com
        * password: Zaq1Xsw2

2. upload a zip file

    navigate to the upload session, and select zip file and fill the other input.

3. make the upload version online

4. download

    download url: https://www.devfdg.net/mp-api/apps/:appId/download

    Sample: Swap-ui download link: https://www.devfdg.net/mp-api/apps/03fdf339-9324-4abf-babb-5e1eaad8d873/download

  <video width="100%" height="100%" controls playsinline>
    <source src="https://static.devfdg.net/static/videos/upload.mov" type="video/mp4" />
  </video>
