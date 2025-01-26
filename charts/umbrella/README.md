# Umbrella Helm Chart

## Overview

The **Umbrella** Helm chart provides a Kubernetes deployment structure for multiple subcharts, including Traefik as an ingress controller and a custom service, `hello-world`, for demonstrating deployments. This chart is designed to manage Kubernetes workloads and services efficiently, leveraging Traefik for ingress routing and flexible service configuration.

### Dependencies
This chart relies on the following dependencies:

1. **Traefik**
   - Version: `^10.30.0`
   - Repository: [https://helm.traefik.io/traefik](https://helm.traefik.io/traefik)

2. **Swissserve** (aliased as `hello-world` and optionally as `local-world` for local testing)
   - Version: `^0`
   - Repository: [https://xander-rudolph.github.io/helm](https://xander-rudolph.github.io/helm)

---

## Installation

### Prerequisites

Ensure you have the following installed:

- Kubernetes cluster (v1.21 or later recommended)
- Helm v3
- Proper credentials for any private Docker repositories

### Installing the Chart

To install the chart with default values:

```bash
helm repo add umbrella https://your-helm-repo-url
helm repo update
helm install my-umbrella umbrella
```

To customize values, create a `values.yaml` file and specify overrides as needed:

```yaml
fullnameOverride: "umbrella"

dockerCredentials:
  username: YOUR_GITHUB_USER
  token: YOUR_GITHUB_TOKEN

hello-world:
  fullnameOverride: hello-world
  namespaceOverride: hello-world-example
  domainPath: hello-example
  image:
    replicaCount: 1
    repository: crccheck/hello-world
    pullPolicy: Always
    containerPort: 8000
    tag: latest

traefik:
  namespaceOverride: traefik
  providers:
    kubernetesCRD:
      namespaces: ['traefik', 'hello-world-example']
```

Then install with your custom values:

```bash
helm install my-umbrella umbrella -f values.yaml
```

---

## Configuration

### Global Configuration

- **`fullnameOverride`**: Overrides the release name for resources.

### Docker Credentials

If your Docker images are hosted in a private registry, you must update the `dockerCredentials` section in the `values.yaml` file:

```yaml
dockerCredentials:
  username: <your-github-username>
  token: <your-personal-access-token>
```

### `hello-world` Configuration

| Key               | Description                                            | Default               |
|-------------------|--------------------------------------------------------|-----------------------|
| `fullnameOverride`| Overrides the full name of the hello-world deployment | `hello-world`         |
| `namespaceOverride`| Overrides the namespace for the hello-world deployment | `hello-world-example` |
| `domainPath`      | Specifies the domain path to expose                   | `hello-example`       |
| `image.repository`| Image repository for hello-world                      | `crccheck/hello-world`|
| `image.pullPolicy`| Image pull policy                                     | `Always`              |
| `image.containerPort`| Container port for the service                     | `8000`                |
| `image.tag`       | Image tag to deploy                                   | `latest`              |

### Traefik Configuration

| Key                       | Description                                               | Default             |
|---------------------------|-----------------------------------------------------------|---------------------|
| `namespaceOverride`       | Namespace for Traefik deployment                          | `traefik`          |
| `providers.kubernetesCRD` | Configures CRD namespaces and allows cross-namespace rules | See values.yaml    |
| `ingressRoute.dashboard`  | Specifies routes to expose Traefik dashboard              | See values.yaml    |

---

## Example Values

Below is an example `values.yaml` file:

```yaml
fullnameOverride: "umbrella"

dockerCredentials:
  username: my-github-username
  token: my-github-token

hello-world:
  fullnameOverride: hello-world
  namespaceOverride: hello-world-example
  domainPath: hello-example
  image:
    replicaCount: 1
    repository: crccheck/hello-world
    pullPolicy: Always
    containerPort: 8000
    tag: latest

traefik:
  namespaceOverride: traefik
  providers:
    kubernetesCRD:
      namespaces: ['traefik', 'hello-world-example']
```

---

## Usage Notes

### Local Testing

To test locally with a custom chart, update the `Chart.yaml` file as follows:

```yaml
- name: swissserve
  version: "^0"
  repository: file://../swissserve
  alias: local-world
```

And reference the `local-world` alias in your `values.yaml`.

### Accessing the Traefik Dashboard

Traefik's dashboard is exposed at `/dashboard` on the `web` entry point. For internal load balancers, ensure proper annotations are added to the `service` section.

---

## Troubleshooting

### Common Issues

1. **Ingress not routing traffic**: Ensure the correct `domainPath` and namespace are configured.
2. **Image pull errors**: Verify your Docker credentials are correct and applied.
3. **Namespace issues**: Check that namespaces specified in the `values.yaml` are created and used.

---

## License

This Helm chart is licensed under the MIT License.

---

## Contributing

Contributions are welcome! Submit a pull request or open an issue in the [GitHub repository](https://github.com/xander-rudolph/helm).

---

## References

- [Traefik Helm Chart Documentation](https://github.com/traefik/traefik-helm-chart)
- [Helm Documentation](https://helm.sh/docs/)