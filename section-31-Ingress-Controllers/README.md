# Ingress Controller
---------

- Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. 
Traffic routing is controlled by rules defined on the Ingress resource.

- An Ingress may be configured to give Services externally-reachable URLs, load balance traffic, terminate SSL / TLS, and offer name-based virtual hosting. 

An Ingress controller is responsible for fulfilling the Ingress, usually with a load balancer, though it may also configure your edge router or additional frontends to help handle the traffic.

- An Ingress does not expose arbitrary ports or protocols. 

Exposing services other than HTTP and HTTPS to the internet typically uses a service of type Service.Type=NodePort or Service.Type=LoadBalancer

![Ingress-Controller](https://d33wubrfki0l68.cloudfront.net/91ace4ec5dd0260386e71960638243cf902f8206/c3c52/docs/images/ingress.svg)

- Ingress-Controller is an ADD-ON in minikube

- Ingress YAML

## ADD ROUTING
-----------

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: basic-routing
spec:
  rules:
    - host: fleetman.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: fleetman-webapp
                port: 
                  number: 80
    
    - host: queue.fleetman.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: fleetman-queue
                port: 
                  number: 8161
```
**REFERENCE:** [https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/](https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/)


## ADD AUTHENTICATION
-------------

reference: [https://kubernetes.github.io/ingress-nginx/examples/auth/basic/](https://kubernetes.github.io/ingress-nginx/examples/auth/basic/)

- On ubuntu intall **htpasswd**
```console
sudo apt install apache2-utils
```

- Create username and Password

```console
htpasswd -c <file-name> <username>
New password:
Re-type new password:

htpasswd -c passwordfile admin
New password: admin
Re-type new password: admin

```

- Convert htpasswd into a secret

```console 
kubectl create secret generic basic-auth --from-file=passwordfile

# secret "basic-auth" created

```

- Examine secret

```console
kubectl get secret basic-auth -o yaml

apiVersion: v1
data:
  passwordfile: YWRtaW46JGFwcjEkU0tycFMuNUkkM2pULng0ckhXeEoxUUNRWUFvNVRqLwo=
kind: Secret
metadata:
  creationTimestamp: "2022-11-08T16:59:40Z"
  name: basic-auth
  namespace: default
  resourceVersion: "66488"
  uid: 40f224dd-1048-4359-bd56-a120570b6fe5
type: Opaque


```

- Using kubectl, create an ingress tied to the basic-auth secret

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-with-auth
  annotations:
    # type of authentication
    nginx.ingress.kubernetes.io/auth-type: basic
    # name of the secret that contains the user/password definitions
    nginx.ingress.kubernetes.io/auth-secret: basic-auth
    # message to display with an appropriate context why the authentication is required
    nginx.ingress.kubernetes.io/auth-realm: 'Authentication Required - foo'
spec:
  ingressClassName: nginx
  rules:
  - host: foo.bar.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service: 
            name: http-svc
            port: 
              number: 80

```