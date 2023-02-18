---
id: mainuc-use-case
title: Mainuc Use Case
---
import Mermaid from '@theme/Mermaid'

## Problem
In order to avoid deployment conflict that might affect other team, main-user-center-ui have to be broken up into individual app. But this brings another problem, before it was a Single-Page-Application now whenever we do a client route it will need to do a hard refresh which completely ruin user experience.

## Solution Proposed
The solution is to introduce a shell app like `mainuc-shell-ui`. We can see the graphic below to see how it works.

<Mermaid chart={`
flowchart TB
    subgraph mainuc-shell-ui /my/*
    shell
    end
    shell-->mainuc-dashboard-ui
    subgraph mainuc-dashboard-ui
    /my/dashboard
    /my/welcome
    /my/coupon
    end
    shell-->mainuc-support-ui
    subgraph mainuc-support-ui
    /user-support/feedback/entry
    /user-support/feedback/submit/:type
    /user-support/feedback/history
    end
`}></Mermaid>

So for each of the sub applications like `mainuc-dashboard-ui`, they need to expose their routes through module-federation. The exposed routes bundle will be consumed by `mainuc-shell-ui`. The routing logic would reside in `mainuc-shell-ui` while the components will be remotely imported. 
