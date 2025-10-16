# kubernetes‑vin

This repo contains Kubernetes manifest for deploying the [goapp](https://github.com/Eivis/goapp) - Hello World application and a configuration for argocd application.

|  File | Describtion  |
|---|---|
| deployment/deployment.yaml  |  Goapp deployment and service manifest |
| application.yaml  |  Argocd application configuration |  

## Prerequisites

- `Docker` installed
- `kubectl` installed
- [minikube](https://minikube.sigs.k8s.io/docs/start/) installed

## Set up on Minikube

1. Start Minikube with docker driver

   ```bash
   minikube start --driver=docker
   ```

2. Install argocd on the cluster:

   As stated in the offcial [documentation](https://argo-cd.readthedocs.io/en/stable/) create `argocd` namespace and install the appplication
   ```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```

3. Verify deployment:

   ```bash
   kubectl get pod -n argocd
   ```
   
4. Access argocd UI
   
   Portforward to local machine
   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8081:443
   ```
   Argocd should now be available on localhost:8081 . You can get default admin password:
   ```bash
   kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
   ```
5. Apply argocd configuration file `application.yaml` 
   ```bash
   kubectl apply -f https://raw.githubusercontent.com/Eivis/kubernetes-vin/refs/heads/main/application.yaml
   ```

6. Check argocd UI for created deployments and services
