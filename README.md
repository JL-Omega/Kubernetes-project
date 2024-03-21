<div align="center">
  # WordPress Deployment on k8s Bare-Metal (NGINX Ingress Controller + MetalLB)
</div>

This repository contains Kubernetes YAML manifests for deploying WordPress on bare-metal infrastructure using an NGINX Ingress controller along with MetalLB for Load Balancing.

## Prerequisites

- Kubernetes cluster running on bare-metal servers.
- Ingress controller deployed in the cluster (In this case, Nginx Ingress controller is used).
- MetalLB installed in the cluster for Load Balancing.
- NFS server for persistent storage.

## Manifests Overview

### Deployment Manifests

#### WordPress Deployment (`wordpress-deploy.yml`)
- Creates a Deployment for WordPress with a single replica.
- Specifies container settings, environment variables for database connection, resource limits, ports, and volume mounts.

#### MySQL Deployment (`mysql-deploy.yml`)
- Defines a Deployment for MySQL database with a single replica.
- Sets container configurations, environment variables for database settings, resource limits, ports, and volume mounts.

### Service Manifests

#### WordPress Service (`wordpress-service.yml`)
- Creates a ClusterIP Service for WordPress to allow internal communication within the cluster.

#### MySQL Service (`mysql-service.yml`)
- Defines a ClusterIP Service for MySQL to enable internal communication within the cluster.

### Ingress Manifest

#### Ingress (`ingress.yml`)
- Configures an Ingress resource to expose WordPress externally.
- Specifies the hostname (`protem.com`) and backend service.

### Secret Manifest

#### Secrets (`mysql-wordpress-secrets.yml`)
- Creates a Secret to store sensitive information like database passwords securely.

### MetalLB Configuration

#### MetalLB IP Address Pool (`metallb-ip-pool.yml`)
- Defines an IP address pool for MetalLB to assign IP addresses from.

#### MetalLB L2 Advertisement (`metallb-l2-advertisement.yml`)
- Configures L2 advertisement for MetalLB.

### Namespace Manifest

#### Namespace (`namespace.yml`)
- Creates a Kubernetes namespace named `wordpress` to isolate resources.

## Installation

1. Ensure the prerequisites are met.
2. Apply the manifests in the following order:
   ```bash
   kubectl apply -f namespace.yml
   kubectl apply -f mysql-deploy.yml
   kubectl apply -f mysql-service.yml
   kubectl apply -f mysql-wordpress-secrets.yml
   kubectl apply -f wordpress-deploy.yml
   kubectl apply -f wordpress-service.yml
   kubectl apply -f ingress.yml
   kubectl apply -f metallb-ip-pool.yml
   kubectl apply -f metallb-l2-advertisement.yml

## Notes
- Adjust the IP address range in metallb-ip-pool.yml according to your network configuration.
- Update hostname in ingress.yml to match your domain.
- Ensure the NFS server IP and paths are correctly configured in volume mounts.
- Secrets should be handled securely. In this example, base64 encoded passwords are used for demonstration purposes. Ensure to use a secure method for managing secrets in production.
Feel free to modify the manifests according to your specific requirements.
For any issues or questions, please raise them in the repository's issue tracker.
---
#### Note: This README assumes familiarity with Kubernetes concepts and YAML syntax. If you need assistance with Kubernetes basics or manifest understanding, refer to [LinkedIn](https://www.linkedin.com/in/jean-luc-mpande-75981a23b/)
