# SynergyChat Kubernetes Deployment

This repository contains the Kubernetes manifests used to deploy **SynergyChat** from a Docker Hub image into a Kubernetes cluster.

The project includes configuration for the main application components and supporting infrastructure, including:

- **Deployments**
- **ConfigMaps**
- **Persistent Volumes / Claims**
- **Services**
- **Gateway API resources**
- **Resource configuration for testing and scaling**

## Overview

SynergyChat is deployed as a set of separate Kubernetes workloads:

- **Web** – the frontend application
- **API** – the backend service
- **Crawler** – background worker/service
- **Test deployments** – used for experimenting with CPU and memory behavior

This repo demonstrates how to organize and deploy a multi-service application on Kubernetes using declarative YAML manifests.

## Repository Structure

```text
.
├── api-configmap.yaml
├── api-deployment.yaml
├── api-httproute.yaml
├── api-pvc.yaml
├── api-service.yaml
├── app-gateway.yaml
├── app-gatewayclass.yaml
├── crawler-configmap.yaml
├── crawler-deployment.yaml
├── crawler-service.yaml
├── testcpu-deployment.yaml
├── testcpu-hpa.yaml
├── testram-configmap.yaml
├── testram-deployment.yaml
├── web-configmap.yaml
├── web-deployment.yaml
├── web-hpa.yaml
├── web-httproute.yaml
└── web-service.yaml
```

## Components
Web
The frontend service for SynergyChat.

Includes:

- Deployment
- ConfigMap
- Service
- HTTPRoute
- HorizontalPodAutoscaler


## API
The backend service that powers the application.

Includes:

- Deployment
- ConfigMap
- PersistentVolumeClaim
- Service
- HTTPRoute


## Crawler
A supporting service for background processing.

Includes:

- Deployment
- ConfigMap
- Service


## Gateway
External traffic is routed into the cluster using Gateway API resources.

Includes:

- GatewayClass
- Gateway
- HTTPRoute resources for web and API traffic


## Features
- Deploys a multi-service application to Kubernetes
- Separates configuration from application containers using ConfigMaps
- Exposes internal services with Kubernetes Services
- Uses Gateway API for ingress-style routing
- Demonstrates persistent storage setup for stateful components
- Includes CPU and RAM test manifests for learning and experimentation
- Includes autoscaling configuration for selected workloads


## Prerequisites
Before deploying, make sure you have:

- A running Kubernetes cluster
- kubectl configured to talk to your cluster
- A Gateway API-compatible controller installed if you want Gateway resources to work
- Access to the SynergyChat Docker image on Docker Hub


## Deployment
Apply the manifests with kubectl:

```
kubectl apply -f .
```

Or apply resources individually:

```
kubectl apply -f app-gatewayclass.yaml
kubectl apply -f app-gateway.yaml

kubectl apply -f api-configmap.yaml
kubectl apply -f api-pvc.yaml
kubectl apply -f api-deployment.yaml
kubectl apply -f api-service.yaml
kubectl apply -f api-httproute.yaml

kubectl apply -f web-configmap.yaml
kubectl apply -f web-deployment.yaml
kubectl apply -f web-service.yaml
kubectl apply -f web-httproute.yaml
kubectl apply -f web-hpa.yaml

kubectl apply -f crawler-configmap.yaml
kubectl apply -f crawler-deployment.yaml
kubectl apply -f crawler-service.yaml
```

## Verify the Deployment
Check running workloads:

```
kubectl get pods
kubectl get deployments
kubectl get services
kubectl get gateways
kubectl get httproutes
```

## Scaling and Resource Testing
This repository also includes manifests for testing Kubernetes resource behavior:

- testcpu-deployment.yaml
- testcpu-hpa.yaml
- testram-deployment.yaml

These can be used to explore:

- Resource requests and limits
- Horizontal Pod Autoscaling
- Scheduling behavior under constrained resources


## Notes
- Configuration values are stored in ConfigMaps where appropriate
- Persistent storage is configured for the API component using a PVC
- Networking is split between internal Services and external Gateway routing
- Some manifests are intended for experimentation and learning, not production use


## Future Improvements
Possible next steps for this project:

- Add Secrets for sensitive configuration
- Add Ingress/Gateway TLS support
- Add readiness and liveness probes
- Add CI/CD for automated deployment
- Add namespace isolation
- Add monitoring and logging