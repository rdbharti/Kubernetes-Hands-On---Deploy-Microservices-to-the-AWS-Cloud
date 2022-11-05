# Requests and Limits
## Requests:
-----------

## 1. Memory
In the pod definition we can specify "ResourseRequest".
It can be a memory request or cpu request.
The request goes under the **containers** definition

```yaml
template: # Template for PODS
    metadata:
      # name: webapp # Now since PODs are managed by ReplicaSet we don't need to mention a name
      labels:
        app: queue
    spec:
      containers:
        - name: queue
          image: richardchesterwood/k8s-fleetman-queue:release2
          resources:
            requests:
              memory: 1500Mi # 1Mi = 1024kb; 1M = 1000K
```

The Cluster keeps a check on the **memory** value, so the **node** don't get over allocated

### 2. CPU

Its similar to Memory allocation request.
CPU values could be 100m (milli-cpu i.e 1000th), 0.1, 0.5, 1, 2 etc

```yaml
template: # Template for PODS
    metadata:
      # name: webapp # Now since PODs are managed by ReplicaSet we don't need to mention a name
      labels:
        app: queue
    spec:
      containers:
        - name: queue
          image: richardchesterwood/k8s-fleetman-queue:release2
          resources:
            requests:
              memory: 1500Mi # 1Mi = 1024kb; 1M = 1000K
              cpu: 1
```