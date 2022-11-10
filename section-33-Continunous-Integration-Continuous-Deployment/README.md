# Continuous Integration and Conitunous Deployment
---------

## What is Continuous Integration?

- Continuous integration is a DevOps software development practice where developers regularly merge their code changes into a central repository, after which automated builds and tests are run. 

- Continuous integration most often refers to the build or integration stage of the software release process and entails both an automation component (e.g. a CI or build service) and a cultural component (e.g. learning to integrate frequently). 

- The key goals of continuous integration are to find and address bugs quicker, improve software quality, and reduce the time it takes to validate and release new software updates.

![https://d1.awsstatic.com/product-marketing/DevOps/continuous_integration.4f4cddb8556e2b1a0ca0872ace4d5fe2f68bbc58.png](https://d1.awsstatic.com/product-marketing/DevOps/continuous_integration.4f4cddb8556e2b1a0ca0872ace4d5fe2f68bbc58.png)


### - Common practices

 - Maintain a code repository
 - Automate the build
 - Make the build self-testing
 - Everyone commits to the baseline every day
 - Every commit (to baseline) should be built
 - Every bug-fix commit should come with a test case
 - Keep the build fast
 - Test in a clone of the production environment
 - Make it easy to get the latest deliverables
 - Everyone can see the results of the latest build
 - Automate deployment


## What is Continuous Deployment?

- Continuous deployment (CD) is a software engineering approach in which software functionalities are delivered frequently and through automated deployments.

- Continuous deployment contrasts with continuous delivery (also abbreviated CD), a similar approach in which software functionalities are also frequently delivered and deemed to be potentially capable of being deployed, but are actually not deployed. 

- As such, continuous deployment can be viewed as a more complete form of automation than continuous delivery

# Running Jenkins inside the cluster (minikube)

1. Starting from scratch.

```console
minikube delete
```

2. Start minikube with 4Gi Memory

```bash
minikube start --memory 4096
```
3. Create an Organisation in github.

**Note:**  Organisation in github allow to group repositories.

4. Fork project repository from GitHub (for this demo only) to the organisation

```htm
https://github.com/fleetman-ci-cd-demo

```

My Organisation Link

[https://github.com/orgs/rdb-fleetman-cicd-demo](https://github.com/orgs/rdb-fleetman-cicd-demo)

5. Setting up basic Jenkins System

There is DockerFile inside Jenkins repo on [https://github.com/orgs/rdb-fleetman-cicd-demo](https://github.com/orgs/rdb-fleetman-cicd-demo), 

we have to build it.

```bash
# Create environment variable to map docker commands for minikube

export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.49.2:2376"
export DOCKER_CERT_PATH="/home/ubuntu/.minikube/certs"
export MINIKUBE_ACTIVE_DOCKERD="minikube"

# Or

eval $(minikube -p minikube docker-env)

```
now "docker command will execute inside minikube"