# Swissserve Helm Chart

## Overview

The **Swissserve** Helm chart provides a modular and configurable solution for deploying services in Kubernetes. It includes options for pod security, resource management, ingress configuration, and integration with Azure services, allowing for flexible deployments in various environments.

## Chart Details

- **Name**: Swissserve
- **Type**: Application

---

## Installation

### Prerequisites

Ensure you have the following installed:

- Kubernetes cluster (v1.21 or later recommended)
- Helm v3
- Proper credentials for any private Docker repositories (if required)

### Installing the Chart

Add the Swissserve Helm repository and install the chart:

```bash
helm repo add swissserve https://your-helm-repo-url
helm repo update
helm install my-service swissserve
```

To customize values, create a `values.yaml` file and override the defaults as needed:

```yaml
image:
  repository: ghcr.io/yourorg/imagename
  pullPolicy: Always
  containerPort: 8080
  tag: "latest"

service:
  type: ClusterIP
  port: 80

azure:
  workloadIdentity: false
  usePodIdentity: false

resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 100m
    memory: 128Mi
```

Install the chart with custom values:

```bash
helm install my-service swissserve -f values.yaml
```

---

## Configuration

### Image Configuration

| Key                 | Description                                  | Default                       |
|---------------------|----------------------------------------------|-------------------------------|
| `repository`        | Docker image repository                     | `ghcr.io/yourorg/imagename`   |
| `pullPolicy`        | Image pull policy                           | `Always`                      |
| `containerPort`     | Container port exposed                      | `8080`                        |
| `tag`               | Image tag                                   | `latest`                      |

### Service Configuration

| Key       | Description                     | Default      |
|-----------|---------------------------------|--------------|
| `type`    | Kubernetes service type         | `ClusterIP`  |
| `port`    | Service port exposed            | `80`         |

### Azure Configuration

| Key                  | Description                                         | Default |
|----------------------|-----------------------------------------------------|---------|
| `workloadIdentity`   | Enable workload identity for Azure authentication   | `false` |
| `usePodIdentity`     | Use Azure Pod Identity                              | `false` |
| `useVMManagedIdentity` | Use VM Managed Identity                           | `false` |

### Autoscaling Configuration

| Key                              | Description                                   | Default |
|----------------------------------|-----------------------------------------------|---------|
| `autoscaling.enabled`            | Enable autoscaling                           | `false` |
| `autoscaling.minReplicas`        | Minimum number of replicas                   | `1`     |
| `autoscaling.maxReplicas`        | Maximum number of replicas                   | `3`     |
| `autoscaling.targetCPUUtilizationPercentage` | Target CPU utilization percentage | `80`    |
| `autoscaling.targetMemoryUtilizationPercentage` | Target memory utilization percentage | `80` |

### Ingress Configuration

| Key               | Description                                           | Default                   |
|-------------------|-------------------------------------------------------|---------------------------|
| `ingress.enabled` | Enable ingress for the service                       | `false`                   |
| `ingress.className` | Ingress class name                                  | `nginx`                   |
| `ingress.domain`  | Domain for the ingress                               | `chart-example.local`     |
| `ingress.path`    | Path for the ingress                                 | `/example-service`        |
| `ingress.annotations` | Annotations for ingress                          | `cert-manager annotations`|

### Resources Configuration

| Key              | Description                   | Default |
|------------------|-------------------------------|---------|
| `resources.limits.cpu`    | Maximum CPU allowed       | `100m` |
| `resources.limits.memory` | Maximum memory allowed    | `128Mi`|
| `resources.requests.cpu`  | Minimum CPU requested     | `100m` |
| `resources.requests.memory`| Minimum memory requested | `128Mi`|

---

## Example Values

Below is an example `values.yaml` file:

```yaml
image:
  repository: ghcr.io/yourorg/imagename
  pullPolicy: Always
  containerPort: 8080
  tag: "latest"

service:
  type: ClusterIP
  port: 80

azure:
  workloadIdentity: false
  usePodIdentity: false

resources:
  limits:
    cpu: 200m
    memory: 256Mi
  requests:
    cpu: 100m
    memory: 128Mi

autoscaling:
  enabled: true
  minReplicas: 2
  maxReplicas: 5
  targetCPUUtilizationPercentage: 75

ingress:
  enabled: true
  domain: app.example.com
  path: /
  annotations:
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
```

---

## Usage Notes

### Image Pull Secrets
If your image repository is private, configure `imagePullSecrets`:

```yaml
imagePullSecrets:
  - name: dockerconfigjson-github-com
```

### Accessing the Service
- The service can be accessed via its `ClusterIP` or ingress (if enabled).
- Configure the `domainPath` in conjunction with Traefik for advanced routing.

---

## License

This Helm chart is licensed under the MIT License.

---

## Contributing

Contributions are welcome! Submit a pull request or open an issue in the [GitHub repository](https://github.com/xander-rudolph/helm).

---

## References

- [Helm Documentation](https://helm.sh/docs/)
- [Azure Kubernetes Service Documentation](https://learn.microsoft.com/en-us/azure/aks/)

