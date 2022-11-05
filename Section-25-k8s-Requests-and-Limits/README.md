# Requests and Limits
## Requests:
-----------
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
              memory: 300Mi # 1Mi = 1024kb; 1M = 1000K
```

