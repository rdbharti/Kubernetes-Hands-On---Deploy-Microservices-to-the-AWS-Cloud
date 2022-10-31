## Queue
--------
**- Image:** richardchesterwood/k8s-fleetman-queue:release1

**- Port 8161:** expose this to a browser using 30010

**- Port 61616:** is used to send/receive messages
<br />

## Position Simulator
----------
**- Image:** richardchesterwood/k8s-fleetman-position-simulator:release1

**- Ports:** No Ports needed - its isolated

It needs an environment variable

SPRING_PROFILES_ACTIVE = production-microservice

## Position Tracker
----------
**- Image:** richardchesterwood/k8s-fleetman-position-tracker:release1

**- Ports: 8080** we can expose port 8080 to test the REST inteface, after testing we will remove the exposed port for all for security reasons.
So we change the "spec.type=ClusterIP" and remove the spec.ports.nodePort number

It needs an environment variable

SPRING_PROFILES_ACTIVE = production-microservice

**- Rest Interface:** /vehicle/{vehicle-name}
    eg: /vehicle/City Truck
        /vehicle/City%20Truck