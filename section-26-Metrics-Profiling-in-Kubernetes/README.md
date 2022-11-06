### How do we find the value of Memory or/and CPU that a particular may need to run comfirtably?

- We can make some rough estimate depending on the underlying technology used.

- Or we can do some profiling of the container using
    - Profiling softwares
    - Or running metrics of the kubernetes cluster

```
# Check how much resource each process is utilising

kubectl top pod

# The above command will throw an error
```
Out-of-the-box this above command does not work, we need to enable metrics server.

**Metric Server** is run in a pod in **kube-system** namespace.

Metric server is an **ADD-ON** for mini-kube

```
# List the add-ons in mini-kube

minikube addons list
```

OUTPUT

|-----------------------------|----------|--------------|--------------------------------|
|         ADDON NAME          | PROFILE  |    STATUS    |           MAINTAINER           |
|-----------------------------|----------|--------------|--------------------------------|
| ambassador                  | minikube | disabled     | 3rd party (Ambassador)         |
| auto-pause                  | minikube | disabled     | Google                         |
| csi-hostpath-driver         | minikube | disabled     | Kubernetes                     |
| dashboard                   | minikube | disabled     | Kubernetes                     |
| default-storageclass        | minikube | enabled ✅   | Kubernetes                     |
| efk                         | minikube | disabled     | 3rd party (Elastic)            |
| freshpod                    | minikube | disabled     | Google                         |
| gcp-auth                    | minikube | disabled     | Google                         |
| gvisor                      | minikube | disabled     | Google                         |
| headlamp                    | minikube | disabled     | 3rd party (kinvolk.io)         |
| helm-tiller                 | minikube | disabled     | 3rd party (Helm)               |
| inaccel                     | minikube | disabled     | 3rd party (InAccel             |
|                             |          |              | [info@inaccel.com])            |
| ingress                     | minikube | disabled     | Kubernetes                     |
| ingress-dns                 | minikube | disabled     | Google                         |
| istio                       | minikube | disabled     | 3rd party (Istio)              |
| istio-provisioner           | minikube | disabled     | 3rd party (Istio)              |
| kong                        | minikube | disabled     | 3rd party (Kong HQ)            |
| kubevirt                    | minikube | disabled     | 3rd party (KubeVirt)           |
| logviewer                   | minikube | disabled     | 3rd party (unknown)            |
| metallb                     | minikube | disabled     | 3rd party (MetalLB)            |
|<span style="color:green"> **metrics-server** </span>             | <span style="color:green"> **minikube** </span> | <span style="color:green"> **disabled** </span>     | <span style="color:green"> **Kubernetes** </span>                |
| nvidia-driver-installer     | minikube | disabled     | Google                         |
| nvidia-gpu-device-plugin    | minikube | disabled     | 3rd party (Nvidia)             |
| olm                         | minikube | disabled     | 3rd party (Operator Framework) |
| pod-security-policy         | minikube | disabled     | 3rd party (unknown)            |
| portainer                   | minikube | disabled     | 3rd party (Portainer.io)       |
| registry                    | minikube | disabled     | Google                         |
| registry-aliases            | minikube | disabled     | 3rd party (unknown)            |
| registry-creds              | minikube | disabled     | 3rd party (UPMC Enterprises)   |
| storage-provisioner         | minikube | enabled ✅   | Google                         |
| storage-provisioner-gluster | minikube | disabled     | 3rd party (Gluster)            |
| volumesnapshots             | minikube | disabled     | Kubernetes                     |
|-----------------------------|----------|--------------|--------------------------------|


### Enable ADDONS

```
minikube addons enable <addon-name>

minikube addons enable metrics-server
```

### Check if the pod started

```
kubectl get po -n kube-system
```

### Check memory and CPU usage of pods

```
kubectl top pod

kubectl top nodes
```
The above command displays the actual usage of the pods

OUTPUTS
```
kubectl top pods

NAME                                CPU(cores)   MEMORY(bytes)
api-gateway-565c8985fb-58f27        23m          708Mi
mongodb-65f4f67c6d-zb8t4            92m          179Mi
position-simulator-f9f545ff-6zww5   47m          577Mi
position-tracker-7fc85bb948-ng4nh   28m          678Mi
queue-5768444bf6-vh2kl              54m          288Mi
webapp-5f98779747-8fbmd             0m           9Mi
webapp-5f98779747-bjrf8             0m           8Mi
webapp-5f98779747-lwllw             0m           9Mi
```

```
kubectl top node

NAME       CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
minikube   684m         17%    3384Mi          58%

```

### To view the metrics in a Dashboard

There is an addon in kubernetes for dashboard

```
# Enable dashboard

minikube addons enable dashboard

# View dashboard pod running status

kubectl get pod -n kube-system

# Access the dashboard

minikube dashboard
```