# Stateless WebApp Kubernetes Helm Chart

This repository contains a **Helm chart** to deploy a stateless web application (Nginx/Node.js) on Kubernetes. It follows **enterprise-grade standards** with configurable resources, Horizontal Pod Autoscaler (HPA), and optional ServiceAccount and Ingress.

---

## **Table of Contents**

- [Features](#features)  
- [Prerequisites](#prerequisites)  
- [Installation](#installation)  
- [Configuration](#configuration)  
- [Usage](#usage)  
- [Verify Deployment](#verify-deployment)  
- [Best Practices](#best-practices)  
- [License](#license)  

---

## **Features**

- Stateless web app deployment with **3 replicas**  
- Configurable **environment variables** via Helm values  
- **ClusterIP Service** to expose the app internally  
- **Horizontal Pod Autoscaler (HPA)** scaling between 3–10 pods based on CPU > 70%  
- Configurable **ServiceAccount** for RBAC  
- Optional **Ingress** support  
- Enterprise-ready Helm templates with environment overrides  

---

## **Prerequisites**

- Kubernetes **v1.25+** cluster  
- **Helm v3.12+**  
- `kubectl` configured to access your cluster  

---

## **Installation**

1. Clone the repository:

```bash
git clone https://github.com/<username>/stateless-webapp-k8s.git
cd stateless-webapp
```

2. Install the Helm chart:

```bash
helm install webapp ./stateless-webapp
```

3. (Optional) Uninstall:

```bash
helm uninstall webapp
```

---

## **Configuration**

All configuration is managed in `values.yaml`. Key parameters:

```yaml
replicaCount: 3

image:
  repository: nginx
  tag: alpine
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80

resources:
  limits:
    cpu: 500m
    memory: 256Mi
  requests:
    cpu: 100m
    memory: 128Mi

env:
  APP_ENV: prod

hpa:
  enabled: true
  minReplicas: 3
  maxReplicas: 10
  targetCPUUtilizationPercentage: 70

serviceAccount:
  create: true
  name: ""

ingress:
  enabled: false
  annotations: {}
  hosts:
    - host: chart-example.local
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls: []
```

You can override these values with a custom file:

```bash
helm install webapp ./stateless-webapp -f values-prod.yaml
```

---

## **Usage**

- Access the service internally using ClusterIP:

```bash
kubectl get svc
kubectl port-forward svc/webapp 8080:80
curl http://localhost:8080
```

- HPA scales automatically based on CPU usage:

```bash
kubectl get hpa
```

- Optional Ingress:

Enable in `values.yaml` or with `--set ingress.enabled=true` and configure hosts.

---

## **Verify Deployment**

```bash
kubectl get deployments
kubectl get pods
kubectl get svc
kubectl get hpa
kubectl describe deployment webapp
```

---

## **Best Practices**

- Define **resource requests/limits** for proper HPA scaling  
- Use **ConfigMaps** for environment variables, avoid hardcoding  
- Follow **Helm best practices**: values.yaml overrides, environment-specific files  
- Consistent **labels** for Deployment, Service, HPA  

---

## **License**

MIT License

