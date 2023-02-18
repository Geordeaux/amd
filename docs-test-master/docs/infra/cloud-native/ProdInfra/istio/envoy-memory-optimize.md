# envoy momory optimize

## namespace isolation
```
apiVersion: networking.istio.io/v1alpha3
kind: Sidecar
metadata:
  name: default
spec:
  egress:
  - hosts:
    - "./*"
    - "istio-system/*" 
```


- [envoy-memory-optimize](https://www.servicemesher.com/blog/201911-envoy-memory-optimize/). We need to consider how to optimize and reduce the additional memory consumption caused by the service grid.
- [sidecar](https://istio.io/latest/docs/reference/config/networking/sidecar/). Sidecar describes the configuration of the sidecar proxy that mediates inbound and outbound communication to the workload instance it is attached to. By default, Istio will program all sidecar proxies in the mesh with the necessary configuration required to reach every workload instance in the mesh, as well as accept traffic on all the ports associated with the workload. The Sidecar configuration provides a way to fine tune the set of ports, protocols that the proxy will accept when forwarding traffic to and from the workload. In addition, it is possible to restrict the set of services that the proxy can reach when forwarding outbound traffic from workload instances.

