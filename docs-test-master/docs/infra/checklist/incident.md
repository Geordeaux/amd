---
id: incident 
title: Incident Checklist
---

import Mermaid from '@theme/Mermaid'

<Mermaid chart={`
flowchart LR
    C[CDN] --> N[nginx]
    N --> I[istio-ingressgateway]
    I --> V[virtualservice]
    V --> S[ssr-cache]
    S --> D[Deployment]
`}>
</Mermaid>

## CHECKLIST
- CALM DOWN
- [HTTP STATUS CODE 200](#check-http-status-code)
- ISTIO GATEWAY HEALTHY
- [REQUEST FROM ISTIO GATEWAY 200](#check-on-istio-ingressgateway)
- [VIRTUAL SERVICE POINT TO THE RIGHT SERVICE](#check-route-and-virtual-service)
- DEPLOYMENT RUNNING
- [HPA PRESSURE UNDER 80%](#check-hpa-pressure)
- NODE LIMIT
- [CLEAN REDIS CACHE](#clean-redis-cache)

## check http status code

```sh
curl -I https://www.binance.com/foo
```

## check on istio-ingressgateway

check istio-ingressgateway (need to sign in to k8s manager machine and exec into any pod with istio injected):

```sh
kubectl exec -it <pod_name> -c istio-proxy -- bash
```

check status code from the istio-ingressgateway

```sh
curl -I -H 'host: www.binance.com' https://istio-ingressgateway.istio-system.svc.cluster.local/foo
```

## Check route and virtual service

open https://tracing.prod-com.toolsfdg.net to see if the url visit the correct service:

```
Service: istio-ingressgateway
Operation: all
Tags: http.url="http://www.binance.com/foo"
```

Find out overlaps of vs fragments

TBD

## Check HPA pressure

sort by cpu usage

```sh
kubectl get hpa | sort -n -k 3
```

change `maxReplicas` in helm release file:
```yaml
hpa:
  minReplicas: 2 # default
  maxReplicas: 10 # default
```

## Clean redis cache

start a redis pod to debug:

```sh
kubectl run -n default -i --restart=Never --tty --rm redis --image=redis --command bash
```

list redis keys with pattern `accounts-ui*`:

```sh
redis-cli -h redis.fe-master.binance.internal --scan --pattern accounts-ui* 
```

delete all keys with pattern
```sh
<previous cmd> | xargs -d '\n' redis-cli -h redis.fe-master.binance.internal del
```
