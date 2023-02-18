---
id: inner-audio
title: InnerAudio
sidebar_label: InnerAudio
---

## Overview

### Protocols

RPC actions and events that are related to InnerAudioContext.

#### Action: `inner-audio-create`

> It is a sync action.

To create inner-audio instance with initial properties.

`payload`

| Property | Type     | Default Value | Required | Description                      |
| -------- | -------- | ------------- | -------- | -------------------------------- |
| props    | `object` |               | ✓        | Partial of InnerAudio properties |

`response`

| Property | Type     | Description |
| -------- | -------- | ----------- |
| id       | `string` | Instance id |

#### Action: `inner-audio-update`

To update inner-audio instance with new properties.

`payload`

| Property | Type     | Default Value | Required | Description                      |
| -------- | -------- | ------------- | -------- | -------------------------------- |
| id       | `string` |               | ✓        | Instance id                      |
| props    | `object` |               | ✓        | Partial of InnerAudio properties |

`response`

#### Action: `inner-audio-get`

To get value of properties inner-audio.

`payload`

| Property | Type     | Default Value | Required | Description                  |
| -------- | -------- | ------------- | -------- | ---------------------------- |
| id       | `string` |               | ✓        | Instance id                  |
| props    | `array`  |               | ✓        | Key of InnerAudio properties |

`response`

| Property          | Type                | Required | Description                      |
| ----------------- | ------------------- | -------- | -------------------------------- |
| key of properties | value of properties |          | Partial of InnerAudio properties |

- example

```json
// response
{
  "payload": {
    "currentTime": 60,
    "duration": 2.115918
  }
}
```

#### Action: `inner-audio-destroy`

To destroy inner-audio instance.

`payload`

| Property | Type     | Default Value | Required | Description |
| -------- | -------- | ------------- | -------- | ----------- |
| id       | `string` |               | ✓        | Instance id |

`response`

#### Action: `inner-audio-invoke`

To invoke inner-audio instance method.

`payload`

| Property | Type     | Default Value | Required | Description                |
| -------- | -------- | ------------- | -------- | -------------------------- |
| id       | `string` |               | ✓        | Instance id                |
| name     | `string` |               | ✓        | method name                |
| data     | `object` |               |          | arguments of invoke method |

`response`

#### Event: `inner-audio-event`

| Property | Type     | Default Value | Required | Description        |
| -------- | -------- | ------------- | -------- | ------------------ |
| id       | `string` |               | ✓        | Instance id        |
| name     | `string` |               | ✓        | Name of the event. |
| data     | `object` |               |          | Data of the event. |

#### Action: `inner-audio-set-all`

Set to take effect globally for the current Mini Program.

`payload`

| Property       | Type      | Default Value | Required | Description                                                                                                          |
| -------------- | --------- | ------------- | -------- | -------------------------------------------------------------------------------------------------------------------- |
| mixWithOther   | `boolean` | true          |          | Whether to mix with other audio. Set to true, after that, the music in other apps or Binance will not be terminated. |
| obeyMuteSwitch | `boolean` | true          |          | (iOS only) Whether to follow the mute switch. Set to false, after that, the sound can be played even in silent mode. |
| speakerOn      | `boolean` | true          |          | `true` Stand for playing on speaker. `false` Represents handset playback.                                            |

`response`

| Property | Type | Description |
| -------- | ---- | ----------- |

### Valid properties

| Property       | Type                          | Readonly |
| -------------- | ----------------------------- | -------- |
| autoplay       | boolean                       |          |
| loop           | boolean                       |          |
| obeyMuteSwitch | boolean                       |          |
| src            | string                        |          |
| startTime      | number                        |          |
| volume         | number                        |          |
| playbackRate   | number                        |          |
| referrerPolicy | `"", "origin", "no-referrer"` |          |
| duration       | number                        | ✓        |
| currentTime    | number                        | ✓        |
| paused         | boolean                       | ✓        |
| buffered       | number                        | ✓        |

#### referrerPolicy

- `origin`: Send the full referrer. Format fixed to `https://www.servicesbinance.com/{appID}/{appVersion}`
- `''` or `no-referrer`: Do not send.

### Valid methods

Methods that start with `on` and `off` will tell native when to start/stop sending events.

- `onPlay`: start to send `Event: inner-audio-event (name=play)`
- `offPlay`: stop to send `Event: inner-audio-event (name=play)`

| name          | data                   |
| ------------- | ---------------------- |
| onCanplay     |                        |
| onPlay        |                        |
| onPause       |                        |
| onStop        |                        |
| onEnded       |                        |
| onTimeUpdate  |                        |
| onError       |                        |
| onWaiting     |                        |
| onSeeking     |                        |
| onSeeked      |                        |
| offCanplay    |                        |
| offPlay       |                        |
| offPause      |                        |
| offStop       |                        |
| offEnded      |                        |
| offTimeUpdate |                        |
| offError      |                        |
| offWaiting    |                        |
| offSeeking    |                        |
| offSeeked     |                        |
| play          |                        |
| pause         |                        |
| stop          |                        |
| seek          | `{ position: number }` |

### Valid events

| name       | data                                                              |
| ---------- | ----------------------------------------------------------------- |
| canplay    |                                                                   |
| ended      |                                                                   |
| pause      |                                                                   |
| play       |                                                                   |
| seeked     |                                                                   |
| seeking    |                                                                   |
| stop       |                                                                   |
| timeUpdate |                                                                   |
| waiting    |                                                                   |
| error      | `{ errCode: 602403, 602404, 602405, 602406, -1; errMsg: string }` |
