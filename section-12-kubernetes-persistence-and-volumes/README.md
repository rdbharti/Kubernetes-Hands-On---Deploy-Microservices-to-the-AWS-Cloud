#### There is a NEW RELEASE: 'release2'
Please update all images to ':release2'

____New Feature:__ vehicle tracking shows the hostory of each vehicle 

<img src="./image/applicatio%20n-diag.png" width="640">

### Changes for the new requirement
1. update position tracker image to release:3
2. setup a mongodb database pod (fleetman-mongodb)
    we will pull image from official mongo page from "hub.docker.com"
    image : mongo:3.6.5-jessie

{In Production we need to make sure Database is deployed first}

## Persistent Volume
----------------------
In the mongo-stack.yaml in Deployment, inside the container we will map a folder inside the container to a folder in the host machine.
We will use the TAG "volumeMounts"