# Defining and consuming Configuration Data

Its common to encounter an application that evaluates environment variables to control its runtime behaviour.

Environment Variables become a common comodity across multiple Pod manifests within the same namespace, There is no way around copy-pasting the definition.

For that particular reason Kubernetes introduces the concept of configuration data represented by dedicated API resources.

Those Api resources are called ConfigMap and Secret.

Both define a set of key-value pairs and can be injected into a container an environment variable or mounted as a Volume.

**NOTE:** Secret expect the value of each entry to be **Base64** encoded

# Creating ConfigMap
-------------

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

3. Consuming config map as "volumeMount"

```yaml

    spec:
      containers:
        - name: position-simulator
          image: richardchesterwood/k8s-fleetman-position-simulator:release2
          env:
            - name: SPRING_PROFILES_ACTIVE
              value: production-microservice

          volumeMounts:
            - name: database-config-volume # Name of volume Mount
            # Path to Directory or Folder inside the image. This directory will be created automatically
              mountPath: /etc/any/directory/config
      volumes: 
        - name: database-config-volume # Name oof volumeMount
          configMap: 
            name: global-database-config # Name of ConfigMap 

          
```

### Edited configMap.yaml

```yaml
apiVersion: v1
kind: ConfigMap
metadata: 
  name: global-database-config
  namespace: default

data:
  data.properties: |  # This bar will allow to store multiple values in a file data.properties
    database.url=http://dbserver.somewhere.com:3306
    database.password=P@ssword123

# OUTPUT 

cat data.properties

database.url=http://dbserver.somewhere.com:3306
database.password=P@ssword123

```

# Creating Secrets
-----------
A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a Pod specification or in a container image. Using a Secret means that you don't need to include confidential data in your application code.

Secrets can be created independently of the Pods that use them

Secrets are similar to ConfigMaps but are specifically intended to hold confidential data.


```
Caution:

Kubernetes Secrets are, by default, stored unencrypted in the API server's underlying data store (etcd). Anyone with API access can retrieve or modify a Secret, and so can anyone with access to etcd. Additionally, anyone who is authorized to create a Pod in a namespace can use that access to read any Secret in that namespace; this includes indirect access such as the ability to create a Deployment.

In order to safely use Secrets, take at least the following steps:

    Enable Encryption at Rest for Secrets.
    Enable or configure RBAC rules with least-privilege access to Secrets.
    Restrict Secret access to specific containers.
    Consider using external Secret store providers.

For more guidelines to manage and improve the security of your Secrets, refer to Good practices for Kubernetes Secrets.

```