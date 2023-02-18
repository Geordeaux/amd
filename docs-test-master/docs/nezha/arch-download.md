---
id: arch-download
title: download
sidebar_label: download
---


# Architecture and System Design

## Upgrade strategy overview

![Architecture](https://static.devfdg.net/static/mono-static/docs-ui/img/arch_download_strategy.png)


## Key design decisions for open-update strategy

1. There are two directories for storing the downloaded resources(cache_replica:RA && RB).If the app just has a directory to store the resource files, when the user opens a mini-program , they have to wait for the upgrade process to finish(If some resource is upgrading).
	* One directory is used to load pages when the user opens a mini-program.
	* Another directory is used to upgrade the resource.
2. It’ll default to checking cache whenever opening the first page of a project.
	* We just need to check the KV in the local storage.
3. If it finds it uses a local resource to render, we will download the resource in background thread，when the new resource is ready, send this message to JS through JSBridge ,there are two choices:
	* Let user to chose whether need to install now, if yes, reload current Nezha(MP), if no, ignore new resource until exit, we will install slient. 
	* Ignore this message, native will isntall new resource slient until exit.


## Check the resource strategy

there is the [API Overview document](api-mp-overview.md#api_host), and you can request the [download API](api-mp-app.md#get-apimanagementappsappiddownload) to get information that you needed.

**Note**: common.js/vender.js will follow Native(iOS/Android) to launch version in the first stage. 

