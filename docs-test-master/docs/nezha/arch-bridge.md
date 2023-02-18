---
id: arch-bridge
title: Bridge
sidebar_label: Bridge
---

First of all, this document is used by implementors but not end users (developers of 3rd-party businesses)

So the interfaces defined below are **NOT** the public APIs of Nezha JavaScript SDK.

## Terminology

<!--
![bridge](https://static.devfdg.net/static/mono-static/docs-ui/img/bridge/bridge.png)
-->

#### Action

An action is sent from the worker to the native side. Whether an action should be followed by a callback (response) is defined in section [Actions](./rpc-action#actions).

The native side should care about the payload of a message according to the type of the action.

#### Message

A message is sent between the worker and a renderer. The payload of a message is not cared by the native side.

A message is **NEVER** followed by a callback.

#### Event

An event is sent from the native side to the worker with initiatives.

## Native Bridge APIs (Native Code)

The APIs of this part is provided by the native side. The native side should inject the `__NEZHA_BRIDGE__` object to the JavaScript environment by using JSCore/JSContext

For the worker

```tsx
interface __NEZHA_BRIDGE__ {
  postAction(msg: ActionRequest): void
  postMessage(msg: Message, rendererIds?: string /* "," separate list */): void
  postWebViewMessage(msg: Message, webviewIds?: string /* "," separate list */): void
}
```

For renderers

```tsx
interface __NEZHA_BRIDGE__ {
  postAction(msg: ActionRequest): void
  postMessage(msg: Message): void
}
```

For webview in mini program

```tsx
interface __NEZHA_BRIDGE__ {
  postMessage(msg: Message): void
}
```

## Web Bridge APIs (JavaScript Code)

Worker

```tsx
interface __NEZHA_WEB_BRIDGE__ {
  // It is called by the native side to send action callback response to js side
  onCallback(msg: CallbackResponse): void

  onMessage(msg: Message, fromRendererId: number): void

  onWebViewMessage(msg: Message, fromWebviewId: number): void
  // It is called by the native side to take initiative to send notification to the worker
  onEvent(msg: EventMessage): void
}
```

Renderer

```tsx
interface __NEZHA_WEB_BRIDGE__ {
  // It is called by the native side to send action callback response to js side
  onCallback(msg: CallbackResponse): void
  // It is called by the native side to take initiative to send notification to the renderer
  onEvent(msg: EventMessage): void

  onMessage(msg: Message): void
}
```

WebView

```tsx
interface __NEZHA_WEB_BRIDGE__ {
  onMessage(msg: Message): void
}
```

## Interfaces

```tsx
interface ActionRequest {
  // An action name only comprises lowercased letters and slashes `-`
  action: string
  // if no actual payload, then payload should be `{}` (an empty object)
  payload: Object
  // It will lead to a failure response message if `callbackId` is not provided for an action which requires a callback
  // If the action request is sent by the native side, there should be no `callbackId` property.
  callbackId?: number
}

interface CallbackResponse {
  callbackId: number
  // If the native side fails to execute the action, then this property should be provided as the reason.
  // The native side should handle i18n content of error messages
  error?: string
  code?: string // https://docs.google.com/spreadsheets/d/1NWCJjFjNJ-jW-Wr7nk8AFFqMmDUZFi0Tz-r__HdvYU0/edit#gid=0
  payload: Object
}

type Message = Object

interface EventMessage {
  type: string
  payload: Object
}
```

## Process

### Action process

Take `set-navigation-bar-title` for example:

Worker calls an action and send `ActionRequest` to the native side

```js
__NEZHA_BRIDGE__.postAction({
  action: 'set-navigation-bar-title',
  payload: {
    title: 'BNB/USDT',
    rendererId: 3,
  },
  callbackId: 1,
})
```

The native side changes title of the renderer with id `3` to `'BNB/USDT'`.

If the title is changed successfully, then the native side will evaluate the following JavaScript code in the worker.

```js
__NEZHA_WEB_BRIDGE__.onCallback({ callbackId: 1, payload: {} })
```

Otherwise, if there is an error, then the native side will evaluate:

```js
__NEZHA_WEB_BRIDGE__.onCallback({ callbackId: 1, error: 'fails to set title', payload: {} }) // i18n ?
```

### Event process example

#### Renderer -> worker

The renderer whose id is `3` calls:

```js
__NEZHA_BRIDGE__.postMessage({
  foo: 'bar',
})
```

The native side receives the `Message` and evaluate the following JavaScript code in the worker

```js
__NEZHA_WEB_BRIDGE__.onMessage({ foo: 'bar' }, 3)
```

As the native side knows that the id of the renderer is `3`, so the second parameter of the above JavaScript is `3`

#### Worker -> Renderer

The worker sends a message to the renderer of id `3`

```js
__NEZHA_BRIDGE__.postMessage(
  {
    foo: 'bar',
  },
  '3',
)
```

Then the native side should evaluate the following JavaScript code in the renderer of id `3`

```js
__NEZHA_WEB_BRIDGE__.onMessage({ foo: 'bar' })
```
