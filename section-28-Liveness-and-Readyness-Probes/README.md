# READINESS PROBE
-----

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