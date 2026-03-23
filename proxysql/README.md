# ProxySQL

This helm chart installs ProxySQL.

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
helm install telekom-mms/proxysql
```

### CI/CD

To use a specific Helm Chart configure your configuration to use

```bash
helm upgrade --install proxysql proxysql --version 0.3.0 --repo https://telekom-mms.github.io/helm-charts/
```

For more information about installing and using Helm, see the [Helm Docs](https://helm.sh/docs/). For a quick introduction to Charts, see the [Chart Guide](https://helm.sh/docs/topics/charts/).

### Examples

#### Minimal Configuration

```yaml
daemonset:
  containers:
    image: proxysql/proxysql:latest
```

#### Full Configuration

```yaml
name: "proxysql"
namespace: "default"
service:
  ports:
  - protocol: TCP
    port: 6033
  type: "NodePort"
daemonset:
  containers:
    image: proxysql/proxysql:latest
    resources:
      requests:
        memory: "500Mi"
        cpu: "0.3"
      limits:
        memory: "1000Mi"
        cpu: "0.6"
configmap:
  mysql_servers:
    - address: "db.example.com"
      port: 3306
  mysql_users:
    - username: "user"
      password: "password"
```

## Contributing

### Existing Chart

We'd love for you to contribute to an existing Chart that you find provides a useful application or service for Kubernetes.
