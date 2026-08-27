# Typo3

This Helm chart installs TYPO3.

## Deployment Modes

Set `mode` to one of:

| Value | Description |
|-------|-------------|
| `mod_php` | Single Apache + mod_php container (default) |
| `fpm` | Webserver sidecar (nginx) + php-fpm container communicating via a Unix socket |

## Usage

### CI/CD

```bash
helm upgrade --install typo3 typo3 --version 0.4.0 --repo https://telekom-mms.github.io/helm-charts/
```

For more information see the [Helm Docs](https://helm.sh/docs/).

### Examples

#### Minimal Configuration (mod_php)

```yaml
deployment:
  containers:
    image:
      registry: docker.io
      repository: myorg/typo3
      tag: "13.4"
```

#### FPM Mode

```yaml
mode: fpm

webserver:
  image:
    registry: docker.io
    repository: nginx
    tag: "1.25"

deployment:
  containers:
    image:
      registry: docker.io
      repository: myorg/typo3-fpm
      tag: "13.4"

phpfpm:
  socketPath: /run/php-fpm/php-fpm.sock
```

#### Environment Variables

Plain env vars are a key/value map. Secret-backed vars use the `secretEnv` list.

```yaml
deployment:
  containers:
    env:
      TYPO3_CONTEXT: Production
      DB_HOST: mysql.example.com
    secretEnv:
      - name: DB_PASSWORD
        secretKeyRef:
          name: typo3-db-secret
          key: password
```

#### Lifecycle Hooks

`lifecycle` must be provided as a YAML string block:

```yaml
deployment:
  containers:
    lifecycle: |
      postStart:
        exec:
          command: ["/bin/sh", "-c", "echo started"]
```

#### Extra Volume Mounts

```yaml
deployment:
  containers:
    volumeMounts:
      extra:
        my-config: /var/www/html/config/my-config
```

#### HTTPRoute (Gateway API)

```yaml
httpRoute:
  enabled: true
  parentRefs:
    - name: my-gateway
      namespace: gateway
  hostnames:
    - typo3.example.com
```

#### Full Configuration

```yaml
name: typo3
namespace: production
replicas: 2
mode: fpm

imagePullSecrets:
  - myregistrysecret

webserver:
  image:
    registry: docker.io
    repository: nginx
    tag: "1.25"
  resources:
    requests:
      memory: "64Mi"
      cpu: "0.1"
      ephemeral_storage: "256Mi"
    limits:
      memory: "128Mi"
      cpu: "0.2"
      ephemeral_storage: "256Mi"

phpfpm:
  socketPath: /run/php-fpm/php-fpm.sock

deployment:
  containers:
    image:
      registry: docker.io
      repository: myorg/typo3-fpm
      tag: "13.4"
    resources:
      requests:
        memory: "512Mi"
        cpu: "0.5"
        ephemeral_storage: "1500Mi"
      limits:
        memory: "1Gi"
        cpu: "1.0"
        ephemeral_storage: "1500Mi"
    env:
      TYPO3_CONTEXT: Production
    secretEnv:
      - name: DB_PASSWORD
        secretKeyRef:
          name: typo3-db-secret
          key: password

persistentvolume:
  enabled: true
  name: typo3-pv
  capacity:
    storage: "20Gi"
  accessModes:
    - ReadWriteMany
  storageClassName: ""
  reclaimPolicy: Retain
  volumeSource: |
    nfs:
      server: nfs-server.example.com
      path: /exports/typo3

persistentvolumeclaim:
  resources:
    requests:
      storage: 20Gi

httpRoute:
  enabled: true
  parentRefs:
    - name: my-gateway
      namespace: gateway
  hostnames:
    - typo3.example.com
```

## Contributing

### Existing Chart

We'd love for you to contribute to an existing Chart that you find provides a useful application or service for Kubernetes.
