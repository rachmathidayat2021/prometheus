# prometheus

Prometheus provides a Kubernetes setup for collecting metrics and viewing them in Grafana. It creates a `monitoring` namespace and deploys the components needed for a basic Prometheus and Grafana setup.

## Features

- Create the `monitoring` namespace with the supplied Kubernetes manifests.
- Deploy Prometheus and Grafana in the monitoring namespace.
- Import the default Grafana dashboards with a Kubernetes Job.
- Forward Grafana to a local port for browser access.
- Add Grafana dashboards that use Prometheus as a data source.

## Installation

This project uses Mustache templates and Kubernetes manifests. From a checkout of the repository, apply the generated all-in-one manifest:

```bash
kubectl apply \
  --filename ./manifests-all.yaml
```

The command creates the `monitoring` namespace and deploys the setup there. To remove the installation:

```bash
kubectl delete namespace monitoring
```

## Usage

Apply the setup and check the resulting pods:

```bash
kubectl apply --filename ./manifests-all.yaml
kubectl --namespace monitoring get pods
```

Import the default Grafana dashboards:

```bash
kubectl apply --filename ./manifests/grafana/import-dashboards/job.yaml
```

If the import Job already exists, delete it first:

```bash
kubectl --namespace monitoring delete job grafana-import-dashboards
```

Forward Grafana to port `3000` on your workstation:

```bash
kubectl port-forward --namespace monitoring service/grafana 3000:3000
```

Open `http://localhost:3000/login` in a browser. For an additional dashboard, configure a Grafana Prometheus data source with the URL `http://prometheus:9090`.

## License

This project is licensed under the Apache License 2.0. It is based on [giantswarm/prometheus](https://github.com/giantswarm/prometheus), also licensed under Apache-2.0. See [LICENSE](LICENSE) for the license text and attribution details.
