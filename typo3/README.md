# Typo3

This helm chart installs Typo3.

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
helm install telekom-mms/typo3
```

### CI/CD

To use a specific Helm Chart configure your configuration to use

```bash
helm upgrade --install typo3 typo3 --version 0.3.0 --repo https://telekom-mms.github.io/helm-charts/
```

For more information about installing and using Helm, see the [Helm Docs](https://helm.sh/docs/). For a quick introduction to Charts, see the [Chart Guide](https://helm.sh/docs/topics/charts/).

### Examples

#### Minimal Configuration

```yaml
deployment:
  containers:
    image:
      registry: docker.io
      repository: martin/typo3
      tag: 11.5
```

#### Full Configuration

```yaml
replicas: 3
deployment:
  containers:
    image:
      registry: docker.io
      repository: martin/typo3
      tag: 11.5
    resources:
      requests:
        memory: "512Mi"
        cpu: "0.5"
      limits:
        memory: "1Gi"
        cpu: "1.0"
    additionalContainers:
      - name: sidecar
        image: busybox
        command: ["sh", "-c", "echo sidecar running; sleep 3600"]
persistentvolume:
  nfs:
    server: "nfs.example.com"
    path: "/export/typo3"
```

## Contributing

### Existing Chart

We'd love for you to contribute to an existing Chart that you find provides a useful application or service for Kubernetes.
