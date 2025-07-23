# Klovercloud Agent Operator

## TL;DR

```bash
helm repo add klovercloud-charts https://klovercloud.github.io/klovercloud-charts/charts

helm repo update

helm install kc-agent-operator --namespace klovercloud klovercloud-charts/klovercloud-agent-operator --version 1.1.0 \
    --set cluster.volumes.storageType="EKS" \
    --set cluster.volumes.storageClass.readWriteMany="csi-obs" \
    --set cluster.volumes.storageClass.readWriteOnce="csi-ebs" \
    --set cluster.volumes.snapshotClass.name="ebs-snapshot" \
    --set platform.service.domain.wildcard.name="abc.com" \
    --set platform.service.webapp.domain="webapp.klovercloud.com" \
    --set platform.service.facade.domain="api.klovercloud.com" \
    --set platform.service.listener.domain="listener.klovercloud.com" \
    --set platform.service.multiClusterConsoleGateway.domain="gateway.klovercloud.com" \
    --set platform.service.facade.webSocketEndpoint="wss://api.klovercloud.com/web-socket-ns"
```

using parameters

To install the operator with the release name  `kc-agent-operator`:

## Uninstallation instructions
```bash
helm delete kc-agent-operator
 ```

# Releases

## Operator chart version

| version | Release Date |
|:--------|:------------:|
| `1.1.0` |   23/07/25   | 
| `1.0.0` |   23/04/25   | 
| `0.2.6` |   08/01/25   | 


## Operator Image Tags

| tag           | Release Type | Release Date |
|:--------------|:------------:|:------------:|
| `v1.2.0-saas` |     SAAS     |   22/07/25   |
| `v1.2.0`      |   Default    |   22/07/25   |


## Parameters

### Operator Parameters

| Name                                                         |                                                                                                                 Description                                                                                                                 | Value                                            | Required |
|:-------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|--------------------------------------------------|:--------:|
| `operator.image.repository`                                  |                                                                                               Klovercloud Operator Public Registry and image                                                                                                | `quay.io/klovercloud/klovercloud-agent-operator` |    ◽     |
| `operator.image.tag`                                         |                                                                                                      Operator image version reference                                                                                                       | `latest`                                         |    ◽     |
| `operator.namespace`                                         |                                                                                                  namespace for the Operator to be deployed                                                                                                  | `"klovercloud"`                                  |    ◽     |
| `operator.klovercloudPlatform.service.facade.apiAccessToken` |                                  Used to access the platform services api's. <br/> This value is required if `platform.user.companyAdmin.email` are `platform.user.companyAdmin.password` is not provided.                                  | `""`                                             |    ◽     |
| `operator.hostAliases`                                       | If operator or platform services requires service Host Aliases. <br/> (e.g. `--set operator.hostAliases[0].ip='34.81.111.16',operator.hostAliases[0].hosts={'api-cp.klovercloud.io,listener-cp.klovercloud.io,gateway-cp.klovercloud.io'}`) | `""`                                             |    ◽     |

### Cluster Parameters

| Name                                           |                                                                Description                                                                 | Value                    | Required |
|:-----------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------:|--------------------------|:--------:|
| `cluster.id`                                   |                                                             Cluster Unique ID                                                              | `""`                     |    ◽     |
| `cluster.type`                                 |                                                Cluster Type (options: `PRIVATE`, `PUBLIC`)                                                 | `"PRIVATE"`              |    ◽     |
| `cluster.host`                                 |                                           Cluster Host (options: `KLOVERCLOUD`, `ROBI`, `BRAC`)                                            | `"KLOVERCLOUD"`          |    ◽     |
| `cluster.creationType`                         |                                           Cluster Creation Type (options: `MANUAL`, `AUTOMATED`)                                           | `"MANUAL"`               |    ◽     |
| `cluster.name`                                 |                                                                Cluster Name                                                                | `""`                     |    ◽     |
| `cluster.volumes.storageType`                  |                        Cluster Volume Storage Type (options: `EKS`, `GCP`, `AZURE`, `DIGITAL_OCEAN`, `BARE_METAL`)                         | `""`                     |    ✅     |
| `cluster.volumes.storageClass.readWriteMany`   |                                                       Cluster Storage Class for RWM                                                        | `""`                     |    ✅     |
| `cluster.volumes.storageClass.readWriteOnce`   |                                                       Cluster Storage Class for RWO                                                        | `""`                     |    ✅     |
| `cluster.volumes.snapshotClass.name`           |                                                        Cluster Volume Snapshot Name                                                        | `""`                     |    ✅     |
| `cluster.volumes.snapshotClassRWM.name`        | Cluster Volume Snapshot Name For ReadWriteMany (FileStorage). If not given, it will take the value of `cluster.volumes.snapshotClass.name` | `""`                     |    ◽     |
| `cluster.clusterIssuer.name`                   |                                             Cluster Issuer Name for SSL/Certificate Management                                             | `""`                     |    ◽     |
| `cluster.serviceAccount.name`                  |                                                    If cluster needs any service account                                                    | `""`                     |    ◽     |
| `cluster.secret.imagePullSecret.name`          |                                                           Image pull secret name                                                           | `""`                     |    ◽     |
| `cluster.notification.webhook.url`             |                                                      Notification Url for all events                                                       | `""`                     |    ◽     |
| `cluster.serviceDns.enabled`                   |                                                DNS enabled or not (options: `true`,`false`)                                                | `""`                     |    ◽     |
| `cluster.volumes.persistentStorage.enabled`    |                                         Storage for saving CD Agent Data (options: `true`,`false`)                                         | `"false"`                |    ◽     |
| `cluster.psp.enforcePrivilegedPsp`             |                                               Force PSP to deploy (options: `True`,`False`)                                                | `"True"`                 |    ◽     |
| `cluster.serviceMesh.istio.enabled`            |                                               Istio enabled or not (options: `true`,`false`)                                               | `"false"`                |    ◽     |
| `cluster.serviceMesh.istio.gateway.name`       |                                                         Istio Default Gateway name                                                         | `""`                     |    ◽     |
| `cluster.serviceMesh.istio.gateway.namespace`  |                                                          Istio gateway namespace                                                           | `"istio-system"`         |    ◽     |
| `cluster.serviceMesh.istio.gateway.labels`     |                                   Istio gateway selector labels  (format: `<key>:<value>;<key>:<value>`)                                   | `"istio:ingressgateway"` |    ◽     |
| `cluster.serviceMesh.istio.clusterIssuer.name` |  Cluster issuer name for istio for SSL/Certificate of Istio gateway. If not given, it will take the value of `cluster.clusterIssuer.name`  | `"istio:ingressgateway"` |    ◽     |

### Platform Parameters

| Name                                                         |                                                                                                                               Description                                                                                                                                | Value           | Required |
|:-------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|-----------------|:--------:|
| `platform.company.id`                                        |                                                                                                   Company ID. Required If value of `cluster.creationType` is `MANUAL`                                                                                                    | `""`            |    ◽     |
| `platform.namespace`                                         |                                                                                                                Namespace where platform will be deployed                                                                                                                 | `"klovercloud"` |    ◽     |
| `platform.service.domain.wildcard.name`                      |                                                 Domain name for Applications External Access through Ingress Controller          (e.g.: 'xyz.com'). <br/> (NOTE: do not provide any protocol such as "http" or "https")                                                  | `""`            |    ✅     |
| `platform.service.domain.wildcard.tlsSecret`                 |                                                                                                          Pre Existed TLS secret for Ingress Controller Domain.                                                                                                           | `""`            |    ◽     |
| `platform.service.domain.ingressClass.name`                  |                                                                                                                    Ingress Class name. (e.g 'nginx')                                                                                                                     | `"nginx"`       |    ◽     |
| `platform.service.domain.ingressController.type`             |                                                                                                    Ingress Controller Type. (options: `NGINX`, `HA_PROXY`, `TRAEFIK`)                                                                                                    | `"NGINX"`       |    ◽     |
| `platform.service.domain.wildcard.tls.enabled`               |                                                                                                     Is tls certificate exists for domain (options: `true`, `false`)                                                                                                      | `"false"`       |    ◽     |
| `platform.service.domain.wildcard.tls.certificate.cert`      |                                                                                                 Domain TLS certificate   (Required If tls certificate exists for domain)                                                                                                 | `""`            |    ◽     |
| `platform.service.domain.wildcard.tls.certificate.key`       |                                                                                                     Domain TLS key   (Required If tls certificate exists for domain)                                                                                                     | `""`            |    ◽     |
| `platform.service.domain.wildcard.tls.certificate.ca`        |                                                                                                        Domain TLS ca   (If you have a custom CA for certificate)                                                                                                         | `""`            |    ◽     |
| `platform.service.domain.wildcard.tls.reflectionEnabled`     |                                                                                             If you want to copy the TLS secret to all namespace  (options: `true`, `false`)                                                                                              | `"false"`       |    ◽     |
| `platform.application.domain.useServiceDomainConfig`         |                                                                            If you want to use the same domain config as service (For Accessing User Applications) (options: `true`, `false`)                                                                             | `"true"`        |    ✅     |
| `platform.application.domain.wildcard.name`                  | Domain name for Applications External Access through Ingress Controller (For Accessing User Applications)   (e.g.: 'xyz.com'). (NOTE: Do not provide any protocol such as "http" or "https"; Required if `platform.application.domain.useServiceDomainConfig` = `false`) | `""`            |    ✅     |
| `platform.application.domain.wildcard.tlsSecret`             |                                                   Pre Existed TLS secret for Ingress Controller Domain. (For Accessing User Applications) (Provide If `platform.application.domain.useServiceDomainConfig` = `false`)                                                    | `""`            |    ◽     |
| `platform.application.domain.ingressClass.name`              |                                                                                                   Ingress Class name. (For Accessing User Applications) (e.g 'nginx')                                                                                                    | `"nginx"`       |    ◽     |
| `platform.application.domain.ingressController.type`         |                                                                                   Ingress Controller Type. (For Accessing User Applications) (options: `NGINX`, `HA_PROXY`, `TRAEFIK`)                                                                                   | `"NGINX"`       |    ◽     |
| `platform.application.domain.wildcard.tls.enabled`           |                                                                                    Is tls certificate exists for domain (For Accessing User Applications) (options: `true`, `false`)                                                                                     | `"false"`       |    ◽     |
| `platform.application.domain.wildcard.tls.certificate.cert`  |                                                                                Domain TLS certificate  (For Accessing User Applications) (Required If tls certificate exists for domain)                                                                                 | `""`            |    ◽     |
| `platform.application.domain.wildcard.tls.certificate.key`   |                                                                                    Domain TLS key  (For Accessing User Applications) (Required If tls certificate exists for domain)                                                                                     | `""`            |    ◽     |
| `platform.application.domain.wildcard.tls.certificate.ca`    |                                                                                        Domain TLS ca (For Accessing User Applications)  (If you have a custom CA for certificate)                                                                                        | `""`            |    ◽     |
| `platform.application.domain.wildcard.tls.reflectionEnabled` |                                                                             If you want to copy the TLS secret to all namespace (For Accessing User Applications) (options: `true`, `false`)                                                                             | `"false"`       |    ◽     |
| `platform.service.serviceMesh.domain.wildcard.name`          |                                                   Domain name for Applications External Access through Istio Gateway Controller (e.g.: 'xyz.com'). <br/> (NOTE: do not provide any protocol such as "http" or "https")                                                   | `""`            |    ◽     |
| `platform.service.serviceMesh.domain.wildcard.tlsSecret`     |                                                                                      TLS secret for Domain name for Applications External Access through Istio Gateway Controller.                                                                                       | `""`            |    ◽     |
| `platform.service.tcp.domain.wildcard.name`                  |                                                              Domain name for Applications External Access through TCP (e.g.: 'xyz.com'). <br/> NOTE: do not provide any protocol such as "http" or "https"                                                               | `""`            |    ◽     |
| `platform.service.tcp.domain.wildcard.tlsSecret`             |                                                                                                 TLS secret for Domain name for Applications External Access through TCP                                                                                                  | `""`            |    ◽     |
| `platform.service.terminal.domain`                           |                                    Klovercloud Terminal Service Domain. If not given, it will be set as `klovercloud-terminal.<service wild card domain>`, (NOTE: service wild card domain = `platform.service.domain.wildcard.name`)                                    | `""`            |    ◽     |
| `platform.service.terminal.wildcard.tlsSecret`               |                                                                                                            TLS secret for Klovercloud Terminal Service Domain                                                                                                            | `""`            |    ◽     |
| `platform.service.lighthouseLogs.domain`                     |                                   Klovercloud Lighthouse Logs Service Domain. If not given, it will be set as `lighthouse-logs.<service wild card domain>`, (NOTE: service wild card domain = `platform.service.domain.wildcard.name`)                                   | `""`            |    ◽     |
| `platform.service.lighthouseLogs.wildcard.tlsSecret`         |                                                                                                              TLS secret for Lighthouse Logs Service Domain                                                                                                               | `""`            |    ◽     |
| `platform.service.terminal.proxy.endpoint`                   |                                                                     If TLS secret not provided for terminal and lighhouse logs domain, then its needed to access terminal and lighthouse logs apis.                                                                      | `""`            |    ◽     |
| `platform.service.webapp.domain`                             |                                                          Klovercloud Webapp Service External Access Domain. (e.g: 'webapp.klovercloud.com') <br/> (NOTE: do not provide any protocol such as "http" or "https")                                                          | `""`            |    ✅     |
| `platform.service.facade.domain`                             |                                                           Klovercloud Facade Service External Access Domain. (e.g: 'api.klovercloud.com') <br/> (NOTE: do not provide any protocol such as "http" or "https")                                                            | `""`            |    ✅     |
| `platform.service.listener.domain`                           |                                                        Klovercloud Listener Service External Access Domain. (e.g: 'listener.klovercloud.com') <br/> (NOTE: do not provide any protocol such as "http" or "https")                                                        | `""`            |    ✅     |
| `platform.service.multiClusterConsoleGateway.domain`         |                                                  Klovercloud Multicliuster Gateway Service External Access Domain. (e.g: 'gateway.klovercloud.com') <br/> (NOTE: do not provide any protocol such as "http" or "https")                                                  | `""`            |    ✅     |
| `platform.service.facade.webSocketEndpoint`                  |                                                                                         Klovercloud Facade Websocket Endpoint. (e.g: 'wss://api.klovercloud.com/web-socket-ns')                                                                                          | `""`            |    ✅     |
| `platform.service.lighthouse.db.purgeEnabled`                |                                                                                                                         Purge DB for Lighthouse                                                                                                                          | `"false"`       |    ◽     |
| `platform.service.agent.logMode`                             |                                                                                               Klovercloud CD Agent Service Log Mode       (options: `DEBUG`, `PRODUCTION`)                                                                                               | `"PRODUCTION"`  |    ◽     |
| `platform.resource.allocation.enabled`                       |                                                                                    Specifying the resource (limit & request) for Klovercloud Services     (options: `true`, `false`)                                                                                     | `"false"`       |    ◽     |
| `platform.resource.allocation.type`                          |                                                                                  Specifying the resource type for Klovercloud Services     (options: `LOW`, `MEDIUM`, `HIGH`, `CUSTOM`)                                                                                  | `"LOW"`         |    ◽     |

### AddOns

| Name                   |                                                      Description                                                      | Value                    | Required |
|:-----------------------|:---------------------------------------------------------------------------------------------------------------------:|--------------------------|:--------:|
| `ci.namespace`         |                                                   Tekton namespaces                                                   | `"kcp-tekton-pipelines"` |    ◽     |
| `ci.tekton.enabled`    |         Tekton is installed or not in cluster (options: `true`,`false`). For installing value will be `true`          | `"true"`                 |    ◽     |
| `loki.url`             |                                    Loki Server Base URl (e.g. "https://loki.com")                                     | `""`                     |    ◽     |
| `loki.username`        |                                                 Loki Server Username                                                  | `""`                     |    ◽     |
| `loki.password`        |                                                 Loki Server Password                                                  | `""`                     |    ◽     |
| `loki.orgId`           |                                        If Loki has tenant then provide the Id                                         | `""`                     |    ◽     |
| `prometheus.enabled`   | Prometheus & Grafana Stack installed or not in cluster (options: `true`,`false`). For installing value will be `true` | `"true"`                 |    ◽     |
| `prometheus.url`       |                              Prometheus Server Base URl (e.g. "https://prometheus.com")                               | `""`                     |    ◽     |
| `prometheus.namespace` |                        Prometheus Namespace. If doesn't exists; it will create new namespace.                         | `"monitoring"`           |    ◽     |
| `prometheus.username`  |                                              Prometheus Server Username                                               | `""`                     |    ◽     |
| `prometheus.password`  |                                              Prometheus Server Password                                               | `""`                     |    ◽     |
| `temporal.host`        |                                       Temporal Host is required for LightHouse                                        | `""`                     |    ◽     |
| `temporal.namespace`   |                                     Temporal Namespace is required for LightHouse                                     | `""`                     |    ◽     |
| `grafana.url`          |                                 Grafana Server Base URl (e.g. "https://grafana.com")                                  | `""`                     |    ◽     |
| `grafana.username`     |                                                Grafana Server Username                                                | `""`                     |    ◽     |
| `grafana.password`     |                                                Grafana Server Password                                                | `""`                     |    ◽     |
| `kiali.url`            |                                   Kiali Server Base URl (e.g. "https://kiali.com")                                    | `""`                     |    ◽     |
| `kiali.username`       |                                                 Kiali Server Username                                                 | `""`                     |    ◽     |
| `kiali.token`          |                                                  Kiali Server Token                                                   | `""`                     |    ◽     |
| `argocd.url`           |                                                      ArgoCD URL                                                       | `""`                     |    ◽     |
| `argocd.username`      |                                                    ArgoCD Username                                                    | `""`                     |    ◽     |
| `argocd.password`      |                                                    ArgoCD Password                                                    | `""`                     |    ◽     |
| `argocd.port`          |                                                      ArgoCD Port                                                      | `""`                     |    ◽     |
