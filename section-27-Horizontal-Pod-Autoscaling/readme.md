# Pod Autoscaling
-------

1. Not every pod is able to be replicated.
2. Pods (the microservice) which can funtion stand-alone can only be replicated i.e Pods does not replicate data on being replicated.
3. Generally Replicated Pod save its data in a database.

## Horizontal Pod AutoScaling (HPA)
We will deploy a new object HPA-rule which will allow to capture a rule.

The rule is along the line of resources utilisation i.e

if the avg_cpu_utilisation > 50% then autoscale.

It is deternined by using the **metric-server**

If avg_cpu_util falls below 50% then replicated pods will be destroyed.

Rules are set individually for each Deployment.

```
# Autoscaling command

kubectl autoscale deployment <deployment-name> --cpu-percent 400 --min 1 --max 4

# --cpu-percent is relative to the resources.requests.cpu
# --min : minimum number of pods
# --max : maximum number of pods

kubectl autoscale deployment api-gateway --cpu-percent 400 --min 1 --max 4

```
### To get HPA 

```
# command

kubectl get hpa

# Output

NAME          REFERENCE                TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
api-gateway   Deployment/api-gateway   10%/200%   1         3         1          16s
```

Copy the hpa object yaml into a file

```
kubectl get hpa api-gateway -o yaml > hpa-api-gateway-dep.yaml
```

Refactored
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: api-gateway
  namespace: default

spec:
  maxReplicas: 3
  metrics:
  - resource:
      name: cpu
      target:
        averageUtilization: 200
        type: Utilization
    type: Resource
  minReplicas: 1
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: api-gateway
```