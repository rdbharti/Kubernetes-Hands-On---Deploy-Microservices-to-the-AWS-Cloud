# Defining and consuming Configuration Data

Its common to encounter an application that evaluates environment variables to control its runtime behaviour.

Environment Variables become a common comodity across multiple Pod manifests within the same namespace, There is no way around copy-pasting the definition.

For that particular reason Kubernetes introduces the concept of configuration data represented by dedicated API resources.

Those Api resources are called ConfigMap and Secret.

Both define a set of key-value pairs and can be injected into a container an environment variable or mounted as a Volume.

**NOTE:** Secret expect the value of each entry to be **Base64** encoded

# ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata: 
  name: global-database-config
  namespace: default

data:
  database.url: "http://dbserver.somewhere.com:3306"
  databasePassword: "P@ssword123"
  ```

### Way to refer/call ConfigMap Data

1. Consuming config map as environment variable

```yaml


  spec:
      containers:
        - name: position-simulator
          image: richardchesterwood/k8s-fleetman-position-simulator:release2
          env:
            - name: SPRING_PROFILES_ACTIVE
              value: production-microservice

            - name: DATABASE_URL
              valueFrom:
                configMapKeyRef:
                  name: global-database-config
                  key: database.url

            - name: DATABASE_PASSWORD
              valueFrom:
                configMapKeyRef:
                  name: global-database-config
                  key: databasePassword


```

2. Consuming config map as "envFrom"

```yaml

# This pulls all of the key-value pair from ConfigMap to environment varibale

spec:
      containers:
        - name: position-simulator
          image: richardchesterwood/k8s-fleetman-position-simulator:release2
          env:
            - name: SPRING_PROFILES_ACTIVE
              value: production-microservice

            envFrom:
              name: global-database-config # configMap name
```

**NOTE:** Keys in CongifMap should be valid environment variables
    ~~database.url~~ = DATABASE_URL