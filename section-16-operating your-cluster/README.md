Since we are moving to cloud
In the Services i.e service.yaml

edit
```
# Under Service WEBAPP

1. remove "nodePort" under "ports"
    because we will use load-balancer and not some high-range ports

2. Change "type" under "spec" to LoadBalancer from NodePort

# Under Service QUEUE

1. remove nodePort
2. chnage type to ClusterIP

```