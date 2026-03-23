# Static Site

This helm chart installs a static site setup.

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
helm install telekom-mms/static_site
```

### CI/CD

To use a specific Helm Chart configure your configuration to use

```bash
helm upgrade --install static_site static_site --version 0.1.1 --repo https://telekom-mms.github.io/helm-charts/
```

For more information about installing and using Helm, see the [Helm Docs](https://helm.sh/docs/). For a quick introduction to Charts, see the [Chart Guide](https://helm.sh/docs/topics/charts/).

### Examples

#### Minimal Configuration

```yaml
deployment:
  containers:
    image:
      registry: docker.io
      repository: nginx
      tag: 1.25.0
```

#### Full Configuration

```yaml
replicas: 2
deployment:
  containers:
    image:
      registry: docker.io
      repository: nginx
      tag: 1.25.0
    resources:
      requests:
        memory: "150Mi"
        cpu: "0.2"
      limits:
        memory: "300Mi"
        cpu: "0.4"
    ports:
      - name: http
        containerPort: 80
```

## Contributing

### Existing Chart

We'd love for you to contribute to an existing Chart that you find provides a useful application or service for Kubernetes.
