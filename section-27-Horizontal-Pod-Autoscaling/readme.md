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