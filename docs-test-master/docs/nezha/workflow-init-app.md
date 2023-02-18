---
id: workflow-init-app
title: Init app
sidebar_label: Init app
---
import Mermaid from '@theme/Mermaid'

## Overview

<Mermaid chart={`
  sequenceDiagram
    Native->>Worker: ①native create and init worker
    Worker->>Native: ②init self and then call ready action
    Worker-->>Native: ③navigate to the first page
    Native->>Renderer: ④create and init renderer
    Renderer->>Worker: ⑤sync message
    Worker->>Renderer: ⑥sync message
`}></Mermaid>
### Action List

- [①init worker](#init-worker)

- [④init renderer](#init-renderer)

## Ready process

### Worker

* Post ready to the native

### Native

* Receive ready from the worker

### Action

When the RPC functions are ready, the worker will send **READY** action by the native bridge to notice the native.

The [$ready](./rpc-action#ready) action will be just sent once.


```ts
// ActionRequest
{
  action: '$ready',
  payload: {}
}  
```

## Init worker

### Inject Global Variables

```ts
interface __NEZHA_APP_CONFIG__ {
  // first url to open
  entryPage: string;
}
```

### Inject Bridge Object

```ts
interface __NEZHA_BRIDGE__ {
  postAction(msg: ActionRequest): void
  postMessage(msg: Message, rendererIds: Array<number>): void
}
```

## Init renderer

### Inject Global Variables

```ts
// the page's url that native get from worker
declare __NEZHA_VIEW_URL__: string
```

### Inject Bridge Object

```tsx
interface __NEZHA_BRIDGE__ {
  postMessage(msg: Message): void
}
```
