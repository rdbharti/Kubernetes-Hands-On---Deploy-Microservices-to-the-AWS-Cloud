# Prometheus and Grafana
------------

We will use pre-configured and pre-tailored stack. [In the Monitoring Folder](./Monitoring/)

###For detail please visit the below link
[https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack](https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack)


Since the Service "Prometheus" is of "type" ClusterIP, it accessible from inside the cluster.
To Access it from outside, we edit the Service

```
kubectl edit svc monitoring-kube-prometheus-prometheus -n monitoring
```
move to bottom of the file
change **type: ClusterIP**
to **type: LoadBalancer**

we can now visit the loadbalncer dns with port 9090