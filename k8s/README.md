# Kubernetes

This helm chart installs generic Kubernetes resources.

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
helm install telekom-mms/k8s
```

### CI/CD

To use a specific Helm Chart configure your configuration to use

```bash
helm upgrade --install k8s k8s --version 0.1.6 --repo https://telekom-mms.github.io/helm-charts/
```

For more information about installing and using Helm, see the [Helm Docs](https://helm.sh/docs/). For a quick introduction to Charts, see the [Chart Guide](https://helm.sh/docs/topics/charts/).

### Examples

#### Minimal Configuration

```yaml
namespace:
  - name: my-namespace
```

#### Full Configuration

```yaml
namespace:
  - name: my-namespace
    labels:
      environment: production

service_account:
  namespace: my-namespace
  account:
    - my-service-account

role:
  - name: my-role
    namespace: my-namespace
    rules:
      - api_groups: [""]
        resources: ["pods"]
        verbs: ["get", "list"]

role_binding:
  - name: my-role-binding
    namespace: my-namespace
    role_ref:
      kind: Role
      name: my-role
      api_group: rbac.authorization.k8s.io
    subjects:
      - kind: ServiceAccount
        name: my-service-account
        namespace: my-namespace

cluster_role:
  - name: my-cluster-role
    rules:
      - api_groups: [""]
        resources: ["nodes"]
        verbs: ["get", "list"]

cluster_role_binding:
  - name: my-cluster-role-binding
    role_ref:
      kind: ClusterRole
      name: my-cluster-role
      api_group: rbac.authorization.k8s.io
    subjects:
      - kind: ServiceAccount
        name: my-service-account
        namespace: my-namespace

secret:
  name: my-secret
  namespace: my-namespace
  type: Opaque
  stringData:
    username: admin
    password: secret-password

cronjob:
  - name: my-cronjob
    namespace: my-namespace
    schedule: "0 0 * * *"
    concurrencyPolicy: Forbid
    failedJobsHistoryLimit: 1
    successfulJobsHistoryLimit: 3
    suspend: false
    backoffLimit: 6
    restartPolicy: OnFailure
    containers:
      image:
        registry: docker.io
        repository: alpine
        tag: 3.14
      imagePullPolicy: IfNotPresent
      command: ["/bin/sh", "-c", "echo hello from cronjob"]
      env:
        MY_ENV: "value"

job:
  - name: my-job
    namespace: my-namespace
    backoffLimit: 4
    restartPolicy: Never
    containers:
      image:
        registry: docker.io
        repository: perl
        tag: 5.34.0
      imagePullPolicy: IfNotPresent
      command: ["perl", "-Mbignum=bpi", "-wle", "print bpi(2000)"]

ingress:
  name: my-ingress
  namespace: my-namespace
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
  rules:
    - host:
        - example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-service
                port:
                  number: 80
```

## Contributing

### Existing Chart

We'd love for you to contribute to an existing Chart that you find provides a useful application or service for Kubernetes.
