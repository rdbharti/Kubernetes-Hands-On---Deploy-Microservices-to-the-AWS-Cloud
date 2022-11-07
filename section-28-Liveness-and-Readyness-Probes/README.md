# READINESS PROBE
-----
- Readiness Probes allows containers to start/bootstrap and when its ready then only traffic is routed to the pod(s)
- Its specified under spec.containers[].

```yaml
spec:
      containers:
        - name: api-gateway
          image: richardchesterwood/k8s-fleetman-api-gateway:performance
          readinessProbe:
            httpGet: # This will send HTTP GET REQUEST to this container
              path: /
              port: 8080 # Internal exposed port of the container image
...

```

# LIVENESS PROBE
--------
- It will continuously run throughout the life of the probe.
- If any of the probe fails, then K8s mark that pod as failed and kill the pod
and restart the pod.