# Telekom MMS helm-charts

This repository contains helm charts for

- Kubernetes configuration
- Installation and / or management of standard applications and 3rd party controllers

## Repository Structure

This GitHub repository contains the source for the packaged and versioned charts released using [GitHub pages](https://github.com/telekom-mms/helm-charts/tree/gh-pages) (the Chart Repository).

The Charts in the root directory in the main branch of this repository match the latest packaged Chart in the Chart Repository, though there may be previous versions of a Chart available in that Chart Repository.

The purpose of this repository is to provide a place for maintaining and contributing Charts, with CI processes [Helm Chart Releaser](https://helm.sh/docs/howto/chart_releaser_action/) in place for managing the releasing of Charts into the Chart Repository.

## Usage

### Client

To add the Helm Charts for your local client, run

```bash
helm repo add telekom-mms https://telekom-mms.github.io/helm-charts/
```

To see available charts and install a chart just run

```bash
# list available charts
helm search repo telekom-mms
# install
helm install telekom-mms/<chart>
```

### CI/CD

To use a specific Helm Chart configure your configuration to use

```bash
helm upgrade --install <name> <chart> --version <version> --repo https://telekom-mms.github.io/helm-charts/


# example k8s charts
helm upgrade --install k8s-config k8s --version 0.1.2 --repo https://telekom-mms.github.io/helm-charts/
```

For more information about installing and using Helm, see the [Helm Docs](https://helm.sh/docs/). For a quick introduction to Charts, see the [Chart Guide](https://helm.sh/docs/topics/charts/).

## Contributing

### Existing Chart

We'd love for you to contribute to an existing Chart that you find provides a useful application or service for Kubernetes.

### New Chart

To add a new Chart please follow the instructions from [Chart Releaser](https://github.com/helm/chart-releaser#usage).
