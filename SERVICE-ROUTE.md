# Services and Routes

## Introduction

In OpenShift, services and routes are used to expose applications to the outside world. Services provide a stable endpoint for accessing pods, while routes manage the external access, including support for both HTTP and HTTPS.

## Key Concepts

- Service: A Service in OpenShift defines how to access a set of pods. It provides a stable DNS name and IP address, and can expose ports for communication. (there is also the concept of headless service but this doc does not cover it.) Service in other terms is also like a load balancer that manages allocation of requests to the backend pods under it.

- Route: A Route in OpenShift exposes a service to the outside world. It handles the routing of external traffic to the service and supports both HTTP and HTTPS, edge termination being one of the secure route mechanism offered.


## Steps to Expose Rover with HTTP and HTTPS

1. Enable HTTP on Rover

Ensure that your Rover is configured to listen on the desired port via http. Do not enable HTTPS. (as it is already being handled by Openshift)

2. Run the deployment

```sh
oc create -f example_objects/rover-ep-deploy.yaml
```

3. Create a service

Define service to expose the Rover within the OpenShift cluster. The Service will map the rover container port to an external port accessible within the cluster.

To create service use the below command:

```sh
oc create -f example_objects/rover-service.yaml
```

4. Create a Route

Define route to expose the service to outside world. This includes setting up the route to handle HTTP and HTTPS traffic.

- HTTP Route: Configures the Route to handle HTTP traffic. By default, HTTP traffic is routed to the Service.

- HTTPS Route with Edge Termination: Configures the Route to handle HTTPS traffic with edge termination. This means that HTTPS connections are terminated at the OpenShift router, which then forwards the traffic to the Service over HTTP.

```sh
oc create -f example_objects/rover-route.yaml
```

### NOTE

- labels are a way of group resources, for instance pods with same labels are grouped under the same service and route.
