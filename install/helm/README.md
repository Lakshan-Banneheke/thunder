# Thunder Helm Chart

This repository contains the Helm chart for WSO2 Thunder, a lightweight user and identity management system designed for modern application development.

## Prerequisites

### Infrastructure
- Running Kubernetes cluster ([minikube](https://kubernetes.io/docs/tasks/tools/#minikube) or an alternative cluster)
- Kubernetes ingress controller ([NGINX Ingress](https://github.com/kubernetes/ingress-nginx) recommended)

### Tools
| Tool          | Installation Guide | Version Check Command |
|---------------|--------------------|-----------------------|
| Git           | [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) | `git --version` |
| Helm          | [Install Helm](https://helm.sh/docs/intro/install/) | `helm version` |
| Docker        | [Install Docker](https://docs.docker.com/engine/install/) | `docker --version` |
| kubectl       | [Install kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl) | `kubectl version` |

## Quick Start Guide

Follow these steps to deploy Thunder in your Kubernetes cluster:

### 1. Clone the Thunder repository

```bash
git clone https://github.com/asgardeo/thunder.git
cd thunder/install/helm
```

### 2. Install the Thunder Helm chart

You can install the Thunder Helm chart with the release name `my-thunder` as follows:

```bash
helm install my-thunder .
```

If you want to customize the installation, create a `custom-values.yaml` file with your configurations and use:

```bash
helm install my-thunder . -f custom-values.yaml
```

The command deploys Thunder on the Kubernetes cluster with the default configuration. The [Parameters](#parameters) section lists the available parameters that can be configured during installation.

### 3. Access Thunder

### 4. Obtain the External IP

After deploying WSO2 Identity Server, you need to find its external IP address to access it outside the cluster. Run the following command to list the Ingress resources:

```bash
kubectl get ingress
```
**Output Fields:**

- **HOSTS** – Hostname (e.g., `thunder.local`)
- **ADDRESS** – External IP
- **PORTS** – Exposed ports (usually 80, 443)

After the installation is complete, you can access Thunder via the Ingress hostname.

By default, Thunder will be available at `http://thunder.local`. You may need to add this hostname to your local hosts file or configure your DNS accordingly.

### Uninstalling the Chart

To uninstall/delete the `my-thunder` deployment:

```bash
helm uninstall my-thunder
```

This command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

The following table lists the configurable parameters of the Thunder chart and their default values.

### Global Parameters

| Name                      | Description                                     | Default                                                 |
| ------------------------- | ----------------------------------------------- | ------------------------------------------------------- |
| `nameOverride`            | String to partially override common.names.fullname | `""`                                                  |
| `fullnameOverride`        | String to fully override common.names.fullname  | `""`                                                    |

### Deployment Parameters

| Name                                    | Description                                                                             | Default                        |
| --------------------------------------- | --------------------------------------------------------------------------------------- | ------------------------------ |
| `deployment.replicaCount`               | Number of Thunder replicas                                                              | `2`                            |
| `deployment.image.registry`             | Thunder image registry                                                                  | `ghcr.io/asgardeo`             |
| `deployment.image.repository`           | Thunder image repository                                                                | `thunder`                      |
| `deployment.image.tag`                  | Thunder image tag                                                                       | `0.7.0`                        |
| `deployment.image.pullPolicy`           | Thunder image pull policy                                                               | `Always`                       |
| `deployment.resources.limits.cpu`       | CPU resource limits                                                                     | `1.5`                          |
| `deployment.resources.limits.memory`    | Memory resource limits                                                                  | `512Mi`                        |
| `deployment.resources.requests.cpu`     | CPU resource requests                                                                   | `1`                            |
| `deployment.resources.requests.memory`  | Memory resource requests                                                                | `256Mi`                        |
| `deployment.securityContext.enableRunAsUser` | Enable running as non-root user                                                    | `true`                         |
| `deployment.securityContext.runAsUser`  | User ID to run the container                                                            | `802`                          |

### HPA Parameters

| Name                              | Description                                                      | Default                       |
| --------------------------------- | ---------------------------------------------------------------- | ----------------------------- |
| `hpa.enabled`                     | Enable Horizontal Pod Autoscaler                                 | `true`                        |
| `hpa.maxReplicas`                 | Maximum number of replicas                                       | `10`                          |
| `hpa.averageUtilizationCPU`       | Target CPU utilization percentage                                | `65`                          |
| `hpa.averageUtilizationMemory`    | Target Memory utilization percentage                             | `75`                          |

### Service Parameters

| Name                             | Description                                                       | Default                      |
| -------------------------------- | ----------------------------------------------------------------- | ---------------------------- |
| `service.port`                   | Thunder service port                                              | `8090`                       |

### Ingress Parameters

| Name                                  | Description                                                     | Default                      |
| ------------------------------------- | --------------------------------------------------------------- | ---------------------------- |
| `ingress.className`                   | Ingress controller class                                        | `nginx`                      |
| `ingress.hostname`                    | Default host for the ingress resource                           | `thunder.local`              |

## Configuration

### Custom Configuration

The Thunder configuration file (deployment.yaml) can be customized by overriding the default configuration in the values.yaml file. 
Alternatively, you can directly update the values in conf/deployment.yaml before deploying the Helm chart.

### Database Configuration

Thunder supports both sqlite and postgres databases. By default, sqlite is configured. You can configure the database connection by overriding the database configuration in the values.yaml file.
