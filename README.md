# pingvin-share-x

A Helm chart for Pingvin Share X

![Version: 1.0.3](https://img.shields.io/badge/Version-1.0.3-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v1.21.0](https://img.shields.io/badge/AppVersion-v1.21.0-informational?style=flat-square)

## Installing the Chart

To install the chart with the release name `pingvin`:

```console
$ helm repo add pingvin-share-x https://viyi.github.io/pingvin-share-x/
$ helm install pingvin pingvin-share-x/pingvin-share-x
```

Declarative config:

```
# Declarative config settings for pingvin, enabling will not allow ui configuration
# https://smp46.github.io/pingvin-share-x/setup/configuration
config:
  enabled: true

  # Option 1: existing config. create a secret in the charts namespace with your config
  # expects a config.yaml to be available
  existingConfig: false

  # Option 2a: Inline config
  # You can define your entire config here or just use it for secrets if you enable useDefault
  inline:
    general:
      appUrl: "http://chart-example.local"

  # Option 2b: apply default config and map inline over it.
  # https://github.com/smp46/pingvin-share-x/blob/main/config.example.yaml
  useDefault: true
```

## Opinionated Declarative Homelab Example (S3 + PocketID oauth)

This is vaguely how I have it set up in my homelab.

```
# values.yaml

env:
  TRUST_PROXY: true

httpRoute:
  enabled: true
  hostnames:
    - pingvin.local
  parentRefs:
  - group: gateway.networking.k8s.io
    kind: Gateway
    name: lan-gateway
    namespace: envoy-gateway-system

config:
  enabled: true

  useDefault: true

  inline:
    general:
      appName: Arthur's Files!
      appUrl: "https://pingvin.local"
      showHomePage: "false"
    share:
      allowRegistration: "false"
      maxSize: "100000000000"
    oauth:
      oidc-enabled: "true"
      #Discovery URI of the OpenID Connect OAuth app
      oidc-discoveryUri: "https://pid.local/.well-known/openid-configuration"
      #Whether the “Sign out” button will sign out from the OpenID Connect provider
      oidc-signOut: "True"
      #Scopes which should be requested from the OpenID Connect provider.
      oidc-scope: openid email profile groups
     
      oidc-rolePath: "groups"
      #Role required for general access. Must be present in a user’s roles for them to log in. Leave it blank if you don't know what this config is.
      oidc-roleGeneralAccess: "pingvin_user"
      #Role required for administrative access. Must be present in a user’s roles for them to access the admin panel. Leave it blank if you don't know what this config is.
      oidc-roleAdminAccess: "pingvin_admin"
    s3:
      #Whether S3 should be used to store the shared files instead of the local file system.
      enabled: "true"
      #The URL of the S3 bucket.
      endpoint: "http://s3.local"
      #The region of the S3 bucket.
      region: "in-my-apartment"
      #The name of the S3 bucket.
      bucketName: "pingvin"
      #The default path which should be used to store the files in the S3 bucket.
      bucketPath: ""
      #Turn off for backends that do not support checksum (e.g. B2).
      useChecksum: "true"
```

Secrets are being managed with [helm-secrets](https://github.com/jkroepke/helm-secrets)

```
# secrets.yaml
config:
  inline:
    oauth:
      oidc-clientId:
      oidc-clientSecret:
    s3:
      key:
      secret:
```

# pingvin-share-x

![Version: 1.0.3](https://img.shields.io/badge/Version-1.0.3-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: v1.21.0](https://img.shields.io/badge/AppVersion-v1.21.0-informational?style=flat-square)

A Helm chart for Pingvin Share X

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` |  |
| config.enabled | bool | `false` |  |
| config.existingConfig | bool | `false` |  |
| config.inline.general.appUrl | string | `"http://chart-example.local"` |  |
| config.useDefault | bool | `true` |  |
| env.CADDY_DISABLED | string | `nil` |  |
| env.CLAMAV_HOST | string | `nil` |  |
| env.CLAMAV_PORT | string | `nil` |  |
| env.PGID | string | `nil` |  |
| env.PUID | string | `nil` |  |
| env.TRUST_PROXY | string | `nil` |  |
| extraEnv | object | `{}` |  |
| fullnameOverride | string | `""` |  |
| httpRoute | object | `{"annotations":{},"enabled":false,"hostnames":["pingvin.local"],"parentRefs":[{"name":"gateway","sectionName":"http"}],"rules":[{"matches":[{"path":{"type":"PathPrefix","value":"/"}}]}]}` | Expose the service via gateway-api HTTPRoute Requires Gateway API resources and suitable controller installed within the cluster (see: https://gateway-api.sigs.k8s.io/guides/) |
| image.pullPolicy | string | `"IfNotPresent"` |  |
| image.repository | string | `"ghcr.io/smp46/pingvin-share-x"` |  |
| image.tag | string | `nil` |  |
| imagePullSecrets | list | `[]` |  |
| ingress.annotations | object | `{}` |  |
| ingress.className | string | `""` |  |
| ingress.enabled | bool | `false` |  |
| ingress.hosts[0].host | string | `"chart-example.local"` |  |
| ingress.hosts[0].paths[0].path | string | `"/"` |  |
| ingress.hosts[0].paths[0].pathType | string | `"ImplementationSpecific"` |  |
| ingress.tls | list | `[]` |  |
| livenessProbe.httpGet.path | string | `"/"` |  |
| livenessProbe.httpGet.port | string | `"http"` |  |
| nameOverride | string | `""` |  |
| nodeSelector | object | `{}` |  |
| persistence.accessMode | string | `"ReadWriteOnce"` |  |
| persistence.enabled | bool | `true` |  |
| persistence.size | string | `"10Gi"` |  |
| persistence.storageClass | string | `""` |  |
| podAnnotations | object | `{}` |  |
| podLabels | object | `{}` |  |
| podSecurityContext | object | `{}` |  |
| readinessProbe.httpGet.path | string | `"/"` |  |
| readinessProbe.httpGet.port | string | `"http"` |  |
| replicaCount | int | `1` |  |
| resources | object | `{}` |  |
| securityContext | object | `{}` |  |
| service.port | int | `3000` |  |
| service.type | string | `"ClusterIP"` |  |
| serviceAccount.annotations | object | `{}` |  |
| serviceAccount.automount | bool | `true` |  |
| serviceAccount.create | bool | `true` |  |
| serviceAccount.name | string | `""` |  |
| tolerations | list | `[]` |  |
| volumeMounts | list | `[]` |  |
| volumes | list | `[]` |  |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.14.2](https://github.com/norwoodj/helm-docs/releases/v1.14.2)
