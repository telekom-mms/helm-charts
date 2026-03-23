# Akv2k8s

This helm chart installs Azure Key Vault to Kubernetes resources.

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
helm install telekom-mms/akv2k8s
```

### CI/CD

To use a specific Helm Chart configure your configuration to use

```bash
helm upgrade --install akv2k8s akv2k8s --version 0.1.4 --repo https://telekom-mms.github.io/helm-charts/
```

For more information about installing and using Helm, see the [Helm Docs](https://helm.sh/docs/). For a quick introduction to Charts, see the [Chart Guide](https://helm.sh/docs/topics/charts/).

### Examples

#### Secret Sync Configuration

```yaml
sync_secret:
  namespace: default
  vault:
    name: my-key-vault
  secret:
    object:
      - my-secret-key
```

#### Full Configuration

```yaml
sync_secret:
  namespace: production
  vault:
    name: main-kv
  secret:
    object:
      - database-password
      - api-key
sync_certificate:
  namespace: production
  vault:
    name: main-kv
  certificate:
    object:
      - ssl-cert
```

## Contributing

### Existing Chart

We'd love for you to contribute to an existing Chart that you find provides a useful application or service for Kubernetes.
