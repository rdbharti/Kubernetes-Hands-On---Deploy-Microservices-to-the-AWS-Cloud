# The Alert Manager

## Receiving Alert on Slack
1. Craete a slack channel
2. add apps -> incoming-webhook
3. check its working by following the instructions on the webhook page

4. Goto Prometheus docs -> Alerting -> Configuration
    [Prometheus-docs-alerting-configuration](https://prometheus.io/docs/alerting/latest/configuration/)
5. On the right hand side look for slack
    read through
6. goto file [alertmanager.yaml](./Alerts%20files/alertmanager.yaml)
7. After making changes in alertmanager.yaml -> Convert it to base64 -> bootstrap it in alertmanager pod
```yaml
kubectl get po -n monitoring

# look for pod starting with alertmanager-*
```
we will use **SECRET** to bootstrap the data.
```yaml
kubectl get secret -n monitoring

# Look for file alertmanager-*

# Now Execute 
kubectl get secret alertmanager-XXXX-DDD -n monitoring -o yaml 
# decode the base64 value under **data**
```

We will first delete that secret and replace it with our secret

```yaml
# Delete Secret
kubectl delete secret alertmanager-XXXX-DDD -n monitoring

# Delete Secret
kubectl create secret generic --from-file=alertmanager.yaml alertmanager-XXXX-DDD -n monitoring
```