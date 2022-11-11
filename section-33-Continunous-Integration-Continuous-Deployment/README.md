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

```console
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
# now "docker command will execute inside minikube"
# Create environment variable to map docker commands for minikube

export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.49.2:2376"
export DOCKER_CERT_PATH="/home/ubuntu/.minikube/certs"
export MINIKUBE_ACTIVE_DOCKERD="minikube"

# Or

eval $(minikube -p minikube docker-env)

# Iamge Build

docker image build -t myjenkins .

## name of the image should be "myjenkins" in nthis case, because this name is used in yaml file

# Confirm Image Build

docker images

## output should have myjenkins under REPOSITORY

docker images

REPOSITORY                                TAG                  IMAGE ID       CREATED          SIZE
myjenkins                                 latest               406ddef68b35   56 seconds ago   885MB
registry.k8s.io/kube-apiserver            v1.25.2              97801f839490   7 weeks ago      128MB
registry.k8s.io/kube-controller-manager   v1.25.2              dbfceb93c69b   7 weeks ago      117MB

```

6. Apply jenkins.yaml file

```console
kubectl apply -f jenkins.yaml
```

7. Access Jenkins

 - [command] kubectl get svc : note down the nodePort of jenkins (31000)
 - [command] minikube ip : 192.168.49.2 (in my case)
 - Open browser and visit : 192.168.49.2:31000

 # Jenkins Pipeline

1. Setup

 Jenkins -> Manage Jenkins -> Configuration System -> Environment Variables (under Global Properties)
    
    ADD: 
        ORGANIZATION_NAME : rdb-fleetman-cicd-demo 
        YOUR_DOCKERHUB_USERNAME : ranadurlabh
    SAVE

2. JenkinsFile under "Fleetman-Api-Gateway" Repo

```json
pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)

     SERVICE_NAME = "fleetman-api-gateway"
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
         }
      }
      stage('Build') {
         steps {
            sh '''mvn clean package'''
         }
      }

      stage('Build and Push Image') {
         steps {
           sh 'docker image build -t ${REPOSITORY_TAG} .'
         }
      }

      stage('Deploy to Cluster') {
          steps {
                    sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}
```

3. MultiBranch PipeLine

    Jenkins -> New Item -> Enter an Item Name: api-gateway -> Multibranch Pipeline

    Display Name: api-gateway
    
    Branch Source -> Github -> ADD -> Jenkins
        Fill Username and password
        and **"ID" should be "GitHub"**

    check Repository Scan - Depricated Visualization
        Owner: Name of Organization = rdb-fleetman-cicd-demo 
        Repository: Drop-Down will list all the repositry under the organization

    Click Save.

This will automatically start the build process.

### Similarly Create a Pipeline for Entire Organization