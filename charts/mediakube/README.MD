### README for Helm Chart

---

# Media Suite Helm Chart

This Helm chart deploys a complete suite of media applications including Transmission, Sonarr, Radarr, Prowlarr, Readarr, Lidarr, Bazarr, Ombi, and EmulatorJS. Each application is pre-configured with essential settings and dependencies for a seamless installation experience in Kubernetes.

---

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
  - [Global Settings](#global-settings)
  - [Application-Specific Settings](#application-specific-settings)
- [Applications Included](#applications-included)
- [Custom Values](#custom-values)
- [Storage](#storage)
- [Secrets and ConfigMaps](#secrets-and-configmaps)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

The Media Suite Helm chart deploys multiple applications for managing and consuming media, offering support for torrent management, media organization, subtitle fetching, and much more. Each application is configurable and includes features such as volume mounting, environment-specific configuration, and persistent storage.

---

## Prerequisites

- Kubernetes cluster (v1.22+ recommended)
- Helm v3+
- Persistent Volume (PV) support for media and configuration storage
- Pre-configured ConfigMaps and Secrets for environment variables

---

## Installation

To install the chart:

```bash
helm repo add media-suite https://your-repo-url
helm repo update
helm install media-suite media-suite/media-suite -f values.yaml
```

---

## Configuration

The chart comes with a `values.yaml` file to customize the deployment. Below are the major configuration categories:

### Global Settings

These settings apply across all deployed applications:

| Parameter           | Description                                | Default                  |
|---------------------|--------------------------------------------|--------------------------|
| `global.configRoot` | Path for configuration storage             | `/mnt/kubestore`       |
| `global.mediaRoot`  | Path for media storage                     | `/mnt/media`             |
| `global.timezone`   | Timezone for all applications              | `Etc/UTC`                |
| `global.PGID`       | Group ID for file permissions             | `0`                      |
| `global.PUID`       | User ID for file permissions              | `0`                      |
| `global.dnsName`    | Internal DNS name for accessing services   | `internal.example.com`   |

### Application-Specific Settings

Each application (e.g., Transmission, Sonarr, etc.) has its own configuration. Here’s an example for Transmission:

```yaml
transmission:
  username: "transmission"
  password: "securepassword"
  download: "/mnt/media/completed"
  vpn:
    username: "vpnUser"
    password: "vpnPassword"
    LOCAL_NETWORK: "10.0.0.0/16"
    OPENVPN_PROVIDER: PIA
    OPENVPN_CONFIG: us_seattle
```

The LOCAL_NETWORK should be specific to your k8s instance and can be determined using the following command:
```
kubectl cluster-info dump | grep cidr
```

Refer to the `values.yaml` file for detailed application-specific settings.

---

## Applications Included

This chart deploys the following applications:

- **Transmission**: Torrent management with VPN support.
- **Sonarr**: TV series manager for Usenet and BitTorrent.
- **Radarr**: Movie manager for Usenet and BitTorrent.
- **Prowlarr**: Indexer manager supporting multiple applications.
- **Readarr**: Book manager for eBooks and audiobooks.
- **Lidarr**: Music manager for Usenet and BitTorrent.
- **Bazarr**: Subtitle management.
- **Ombi**: Media request system.
- **EmulatorJS**: Retro gaming emulator.

---

## Custom Values

To customize the deployment, create your own `custom-values.yaml` file and pass it during installation:

```bash
helm install mediakube xander-rudolph/mediakube -f custom-values.yaml
```

---

## Storage

The chart requires Persistent Volumes (PVs) for storing media and configuration data. By default, the following volumes are expected:

- **Media Volume**: Stores downloaded or managed media (`/mnt/media`).
- **Configuration Volume**: Stores application configuration files (`/mnt/kubestore`).

Update the PVC definitions in `values.yaml` to match your cluster's storage configuration.

---

## Secrets and ConfigMaps

Sensitive data such as API keys, VPN credentials, and database credentials are expected to be stored in Kubernetes Secrets. The chart references these secrets using `envFrom`.

Example:

```yaml
envFrom:
  secretRefs:
    - "transmission-vpn-secret"
  configMapRefs:
    - "general-configmap"
```

These values can be defined as globals or on each specific app. The helm chart will map the values into the appropriate secrets/configmaps.

---

## Contributing

Contributions are welcome! Please open an issue or submit a pull request on GitHub to improve this chart.

---

## License

This Helm chart is licensed under the MIT License. See the LICENSE file for more details.