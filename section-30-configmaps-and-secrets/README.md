# Defining and consuming Configuration Data

Its common to encounter an application that evaluates environment variables to control its runtime behaviour.

Environment Variables become a common comodity across multiple Pod manifests within the same namespace, There is no way around copy-pasting the definition.

For that particular reason Kubernetes introduces the concept of configuration data represented by dedicated API resources.

Those Api resources are called ConfigMap and Secret.

Both define a set of key-value pairs and can be injected into a container an environment variable or mounted as a Volume.

**NOTE:** Secret expect the value of each entry to be **Base64** encoded