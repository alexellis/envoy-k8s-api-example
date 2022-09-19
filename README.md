WIP example

Goal:

Load balance port 6443 (the K8s/K3s API server) against a dynamic list of upstream servers using a file for endpoint discovery.

* Dynamic configuration
* Load balancing port 6443 between a list of endpoints
* TCP Proxy only (`type.googleapis.com/envoy.extensions.filters.network.tcp_proxy.v3.TcpProxy`) - not HTTP/TLS

Commands to test:

```
$ docker run -p 10000:10000 -p 6443:6443 \
  -t -i -v `pwd`:/etc/envoy \
  --entrypoint /bin/bash --rm \
  envoyproxy/envoy-dev:4c0e53d8cee46d9d886ceed011b1a52000d261cf

# cd /etc/envoy
# envoy -l debug -c ./envoy.yaml
```

Other commands:

```
envoy -c ./envoy.yaml   --service-cluster k3s_service  --service-node k3s_service


curl -s http://localhost:10000/config_dump

curl -s http://localhost:10000/config_dump?resource=dynamic_listeners
```
