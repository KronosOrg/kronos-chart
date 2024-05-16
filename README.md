# Kronos Helm Chart

Kronos is a Kubernetes operator designed to manage the wake and sleep cycles of resources based on user-defined schedules. This Helm chart facilitates the deployment of the Kronos operator and its associated components.

## Overview

Kronos helps in optimizing resource usage in Kubernetes environments by allowing users to define schedules for when resources should be awake (active) or asleep (inactive). This can lead to significant cost savings and efficient resource management.

## Installation

To install the Kronos Helm chart, add the repository and use the `helm install` command:

```bash
helm repo add kronos https://kronosorg.github.io/kronos-charts
helm install <release-name> kronos-core/kronos-core --create-namespace true --namespace <installation-namespace> --version 0.4.1 -f values.yaml
```

## Values Schema

The `values.yaml` file allows you to customize the deployment of the Kronos operator. Below is an overview of the schema:

| Key                                      | Type    | Description                                                        |
|------------------------------------------|---------|--------------------------------------------------------------------|
| `manager.resources.limits.cpu`           | string  | Kronos Controller CPU Limits                                       |
| `manager.resources.limits.memory`        | string  | Kronos Controller Memory Limits                                    |
| `manager.resources.requests.cpu`         | string  | Kronos Controller CPU Requests                                     |
| `manager.resources.requests.memory`      | string  | Kronos Controller Memory Requests                                  |
| `metrics.serviceMonitor.create`          | boolean | Enable/Disable Creation of Prometheus ServiceMonitor               |

### Key Values

- **manager.resources.limits**: Specifies the resource limits for the Kronos controller.
  - **cpu**: CPU limit.
  - **memory**: Memory limit.
  
- **manager.resources.requests**: Specifies the resource requests for the Kronos controller.
  - **cpu**: CPU request.
  - **memory**: Memory request.

- **metrics.serviceMonitor.create**: Enables or disables the creation of a Prometheus ServiceMonitor for exposing Kronos controller metrics.

## CRD Configuration

To use Kronos, you need to define `KronosApp` Custom Resource Definitions (CRDs). Below is an example configuration:

```yaml
apiVersion: core.wecraft.tn/v1alpha1
kind: KronosApp
metadata:
  labels:
  name: sleep-at-weekend
spec:
  startSleep: "18:00"
  endSleep: "07:00"
  weekdays: "1-5"
  timezone: "Africa/Tunis"
  includedObjects: 
    - apiVersion: "apps/v1"
      kind: "Deployment"
      namespace: "default"
      includeRef: ""
      excludeRef: ""
```

For more information, refer to the [CRD configuration page](https://kronosorg.github.io/kronos-docs/docs/crd-configuration) of our documentation website.

## Dependencies

- **Cert-Manager**: Required to generate certificates for webhooks.
- **Prometheus and Grafana**: Used to visualize metrics.

## Metrics

Kronos exposes several metrics for monitoring:

- **kronos_schedule_info**: Provides basic schedule information.
- **kronos_indepth_schedule_info**: Provides detailed schedule information including status, reason, handled resources, and next operation.

For visualization, we recommend using the [KronosBoard Grafana dashboard](https://grafana.com/grafana/dashboards/21068-kronosboard/).

## Ecosystem

Kronos is part of a comprehensive suite of tools designed to enhance resource scheduling in Kubernetes:

- **Kronos-Core**: The core operator managing scheduling.
- **KronosCLI**: Provides an interface for managing schedules, giving users the upper hand in handling emergency situations.
- **KronosWebUI**: A web interface for scheduling resources (coming soon).

For more details, visit the [Ecosystem section](https://kronosorg.github.io/kronos-docs/docs/getting-started#eco-system) of our documentation.

## Changelog

### [0.4.0] - 2024-05-16

- Added defaulting and validating webhooks to the Kronos operator.

## Links

- **Documentation**: [Kronos Docs](https://kronosorg.github.io/kronos-docs/)
- **GitHub Repository**: [Kronos GitHub](https://github.com/kronosorg/kronos-core)
- **KronosBoard**: [Grafana Dashboard](https://grafana.com/grafana/dashboards/21068-kronosboard/)

## Contributing

Contributions and improvements are welcomed! If you want to contribute, please open an issue in our [GitHub repository](https://github.com/kronosorg/kronos-core).
