# Xander-Rudolph Helm Charts

Welcome to the **Xander-Rudolph Helm Charts** repository! Here you’ll find a collection of Helm charts designed to simplify and standardize deployments for various applications and use cases.

## Available Charts

### [Grafana](charts/grafana/README.md)
- **Description**: A Helm chart to deploy Grafana with pre-configurations based on folder structures. Perfect for quickly setting up a Grafana instance tailored to your monitoring needs.  
- **Source Repository**: [GitHub - Grafana Chart](https://github.com/Xander-Rudolph/grafana)

### [SwissServe](charts/swissserve/README.md)
- **Description**: A highly flexible and generic Helm chart designed to deploy practically any application or service. It’s a great starting point for custom deployments.  
- **Source Repository**: [GitHub - SwissServe Chart](https://github.com/Xander-Rudolph/swissserve)

### [Umbrella](charts/umbrella/README.md)
- **Description**: A starter template designed to streamline project setups with predefined GitHub authentication tokens and other common configurations. Ideal for new projects needing a strong starting point.  
- **Source Repository**: [GitHub - Umbrella Chart](https://github.com/Xander-Rudolph/umbrella)

### [MediaKube](charts/mediakube/README.md)
- **Description**: A comprehensive Helm chart that bundles the *arr suite of applications (Sonarr, Radarr, etc.) for managing media in Kubernetes. Simplify your media workflow with this pre-configured solution.  
- **Source Repository**: [GitHub - MediaKube Chart](https://github.com/Xander-Rudolph/mediakube)

---

## How to Use These Charts

1. **Add the Repository**  
   Add this Helm repository to your Helm client:
   ```bash
   helm repo add xander-rudolph https://xander-rudolph.github.io/helm
   ```

2. **Search for Charts**  
   List the available charts:
   ```bash
   helm search repo xander-rudolph
   ```

3. **Install a Chart**  
   Install a specific chart by running:
   ```bash
   helm install example-release xander-rudolph/<chart-name>
   ```
   Replace `<chart-name>` with the desired chart (e.g., `grafana`, `swissserve`, `umbrella`, or `mediakube`).

4. **Configuration**  
   Visit the respective chart’s documentation (linked above) for configuration details and examples.

---

## Reporting Issues

If you encounter any issues or have feature requests, please file them in the respective chart’s [source repository](https://github.com/Xander-Rudolph).

---

This format ensures clarity, highlights the purpose of each chart, and makes it easy for users to find and deploy them. Let me know if you’d like additional changes!
