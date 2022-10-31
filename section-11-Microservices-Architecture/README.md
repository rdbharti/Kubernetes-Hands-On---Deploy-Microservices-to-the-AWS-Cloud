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