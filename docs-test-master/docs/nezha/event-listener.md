---
id: event-listener
title: PluginManager - Event Listener
sidebar_label: PluginManager - Event Listener
---

## Overview

### Protocols

RPC actions and events that are related to PluginManager - Event Listener.

#### Action: `framework-event-listener-on`

Start to subscribe specific event.

`payload`

| Property | Type     | Default Value | Required | Description    |
| -------- | -------- | ------------- | -------- | -------------- |
| name     | `string` |               | ✓        | rpc event name |
| payload  | `object` |               |          | event payload  |

`response`

| Property | Type | Description |
| -------- | ---- | ----------- |

#### Action: `framework-event-listener-off`

Unsubscribe specific event.

`payload`

| Property | Type     | Default Value | Required | Description    |
| -------- | -------- | ------------- | -------- | -------------- |
| name     | `string` |               | ✓        | rpc event name |
| payload  | `object` |               |          | event payload  |

`response`

| Property | Type | Description |
| -------- | ---- | ----------- |

#### Event: `framework-event-listener-cancelled`

Fired when the specific event is not available anymore on native, native need to unsubscribe and tell JS.

`payload`

| Property | Type     | Default Value | Required | Description    |
| -------- | -------- | ------------- | -------- | -------------- |
| name     | `string` |               | ✓        | rpc event name |

### Valid RPC event name

- `'audio-interruption-begin'`
- `'audio-interruption-end'`
- `'theme-change'`
- `'location-change'`
- `'network-status-change'`
- `'keyboard-height-chang'`
