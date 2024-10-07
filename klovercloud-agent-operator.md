# Klovercloud Agent Operator

## TL;DR

```bash
helm repo add klovercloud-charts https://klovercloud.github.io/klovercloud-charts/charts

helm repo update

helm install kc-agent-operator --namespace klovercloud klovercloud-charts/klovercloud-agent-operator --version 0.2.6 \
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
| `0.2.6` |   10/09/24   | 
| `0.2.5` |   10/07/24   | 
| `0.2.4` |   04/01/24   | 
| `0.2.3` |   18/05/23   |

## Operator Image Tags

| tag     | Release Date |
|:--------|:------------:|
| `v2.1`  |   10/07/24   | 
| `v2.0`  |   04/01/24   | 
| `v1.99` |   18/05/23   |


## Parameters

### Operator Parameters

| Name                                                         |                                                                                                        Description                                                                                                        | Value                                            | Required |
|:-------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|--------------------------------------------------|:--------:|
| `operator.image.repository`                                  |                                                                                      Klovercloud Operator Public Registry and image                                                                                       | `quay.io/klovercloud/klovercloud-agent-operator` |    ◽     |
| `operator.image.tag`                                         |                                                                                             Operator image version reference                                                                                              | `latest`                                         |    ◽     |
| `operator.namespace`                                         |                                                                                         namespace for the Operator to be deployed                                                                                         | `"klovercloud"`                                  |    ◽     |
| `operator.db.type`                                           |                                                                      Operator DB for caching Operator Data. (options: `CacheControllerDB`,`BuntDb`)                                                                       | `"CacheControllerDB"`                            |    ◽     |
| `operator.db.includePersistentStorage`                       |                                                                     If operator storage information needs to be persisted. (options: `true`,`false`)                                                                      | `"false"`                                        |    ◽     |
| `operator.klovercloudPlatform.service.facade.apiAccessToken` |                         Used to access the platform services api's. <br/> This value is required if `platform.user.companyAdmin.email` are `platform.user.companyAdmin.password` is not provided.                         | `""`                                             |    ◽     |
| `operator.hostAliases`                                       | If operator or platform services requires service Host Aliases. <br/> (e.g. `--set operator.hostAliases[0].ip='34.81.232.16',operator.hostAliases[0].hosts={'api-cp.brac.net,listener-cp.brac.net,gateway-cp.brac.net'}`) | `""`                                             |    ◽     |

### Cluster Parameters

| Name                                         |                                         Description                                         | Value           | Required |
|:---------------------------------------------|:-------------------------------------------------------------------------------------------:|-----------------|:--------:|
| `cluster.id`                                 |                                      Cluster Unique ID                                      | `""`            |    ◽     |
| `cluster.type`                               |                         Cluster Type (options: `PRIVATE`, `PUBLIC`)                         | `"PRIVATE"`     |    ◽     |
| `cluster.host`                               |                    Cluster Host (options: `KLOVERCLOUD`, `ROBI`, `BRAC`)                    | `"KLOVERCLOUD"` |    ◽     |
| `cluster.creationType`                       |                   Cluster Creation Type (options: `MANUAL`, `AUTOMATED`)                    | `"MANUAL"`      |    ◽     |
| `cluster.name`                               |                                        Cluster Name                                         | `""`            |    ◽     |
| `cluster.volumes.storageType`                | Cluster Volume Storage Type (options: `EKS`, `GCP`, `AZURE`, `DIGITAL_OCEAN`, `BARE_METAL`) | `""`            |    ✅     |
| `cluster.volumes.storageClass.readWriteMany` |                                Cluster Storage Class for RWM                                | `""`            |    ✅     |
| `cluster.volumes.storageClass.readWriteOnce` |                                Cluster Storage Class for RWO                                | `""`            |    ✅     |
| `cluster.volumes.snapshotClass.name`         |                                Cluster Volume Snapshot Name                                 | `""`            |    ✅     |
| `cluster.clusterIssuer.name`                 |                     Cluster Issuer Name for SSL/Certificate Management                      | `""`            |    ◽     |
| `cluster.serviceAccount.name`                |                            If cluster needs any service account                             | `""`            |    ◽     |
| `cluster.secret.imagePullSecret.name`        |                                   Image pull secret name                                    | `""`            |    ◽     |
| `cluster.notification.webhook.url`           |                               Notification Url for all events                               | `""`            |    ◽     |
| `cluster.serviceDns.enabled`                 |                        DNS enabled or not (options: `true`,`false`)                         | `""`            |    ◽     |
| `cluster.volumes.persistentStorage.enabled`  |                 Storage for saving CD Agent Data (options: `true`,`false`)                  | `"false"`       |    ◽     |
| `cluster.psp.enforcePrivilegedPsp`           |                        Force PSP to deploy (options: `True`,`False`)                        | `"True"`        |    ◽     |
| `cluster.serviceMesh.istio.enabled`          |                       Istio enabled or not (options: `true`,`false`)                        | `"false"`       |    ◽     |
| `cluster.nginx.ingressClass.name`            |                                     Ingress class name                                      | `""`            |    ◽     |

### Platform Parameters

| Name                                                     |                                                                                             Description                                                                                              | Value           | Required |
|:---------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|-----------------|:--------:|
| `platform.company.id`                                    |                                                                 Company ID. Required If value of `cluster.creationType` is `MANUAL`                                                                  | `""`            |    ◽     |
| `platform.namespace`                                     |                                                                              Namespace where platform will be deployed                                                                               | `"klovercloud"` |    ◽     |
| `platform.service.domain.wildcard.name`                  |               Domain name for Applications External Access through Ingress Controller          (e.g.: 'xyz.com'). <br/> (NOTE: do not provide any protocol such as "http" or "https")                | `""`            |    ✅     |
| `platform.service.domain.wildcard.tlsSecret`             |                                                                              TLS secret for Ingress Controller Domain.                                                                               | `""`            |    ◽     |
| `platform.service.serviceMesh.domain.wildcard.name`      |                 Domain name for Applications External Access through Istio Gateway Controller (e.g.: 'xyz.com'). <br/> (NOTE: do not provide any protocol such as "http" or "https")                 | `""`            |    ◽     |
| `platform.service.serviceMesh.domain.wildcard.tlsSecret` |                                                    TLS secret for Domain name for Applications External Access through Istio Gateway Controller.                                                     | `""`            |    ◽     |
| `platform.service.tcp.domain.wildcard.name`              |                            Domain name for Applications External Access through TCP (e.g.: 'xyz.com'). <br/> NOTE: do not provide any protocol such as "http" or "https"                             | `""`            |    ◽     |
| `platform.service.tcp.domain.wildcard.tlsSecret`         |                                                               TLS secret for Domain name for Applications External Access through TCP                                                                | `""`            |    ◽     |
| `platform.service.terminal.domain`                       |  Klovercloud Terminal Service Domain. If not given, it will be set as `klovercloud-terminal.<service wild card domain>`, (NOTE: service wild card domain = `platform.service.domain.wildcard.name`)  | `""`            |    ◽     |
| `platform.service.terminal.wildcard.tlsSecret`           |                                                                          TLS secret for Klovercloud Terminal Service Domain                                                                          | `""`            |    ◽     |
| `platform.service.lighthouseLogs.domain`                 | Klovercloud Lighthouse Logs Service Domain. If not given, it will be set as `lighthouse-logs.<service wild card domain>`, (NOTE: service wild card domain = `platform.service.domain.wildcard.name`) | `""`            |    ◽     |
| `platform.service.lighthouseLogs.wildcard.tlsSecret`     |                                                                            TLS secret for Lighthouse Logs Service Domain                                                                             | `""`            |    ◽     |
| `platform.service.terminal.proxy.endpoint`               |                                   If TLS secret not provided for terminal and lighhouse logs domain, then its needed to access terminal and lighthouse logs apis.                                    | `""`            |    ◽     |
| `platform.service.webapp.domain`                         |                        Klovercloud Webapp Service External Access Domain. (e.g: 'webapp.klovercloud.com') <br/> (NOTE: do not provide any protocol such as "http" or "https")                        | `""`            |    ✅     |
| `platform.service.facade.domain`                         |                         Klovercloud Facade Service External Access Domain. (e.g: 'api.klovercloud.com') <br/> (NOTE: do not provide any protocol such as "http" or "https")                          | `""`            |    ✅     |
| `platform.service.listener.domain`                       |                      Klovercloud Listener Service External Access Domain. (e.g: 'listener.klovercloud.com') <br/> (NOTE: do not provide any protocol such as "http" or "https")                      | `""`            |    ✅     |
| `platform.service.multiClusterConsoleGateway.domain`     |                Klovercloud Multicliuster Gateway Service External Access Domain. (e.g: 'gateway.klovercloud.com') <br/> (NOTE: do not provide any protocol such as "http" or "https")                | `""`            |    ✅     |
| `platform.service.facade.webSocketEndpoint`              |                                                       Klovercloud Facade Websocket Endpoint. (e.g: 'wss://api.klovercloud.com/web-socket-ns')                                                        | `""`            |    ✅     |
| `platform.service.lighthouse.db.purgeEnabled`            |                                                                                       Purge DB for Lighthouse                                                                                        | `"false"`       |    ◽     |
| `platform.service.agent.logMode`                         |                                                             Klovercloud CD Agent Service Log Mode       (options: `DEBUG`, `PRODUCTION`)                                                             | `"PRODUCTION"`  |    ◽     |
| `platform.resource.allocation.enabled`                   |                                                  Specifying the resource (limit & request) for Klovercloud Services     (options: `true`, `false`)                                                   | `"false"`       |    ◽     |
| `platform.resource.allocation.type`                      |                                                Specifying the resource type for Klovercloud Services     (options: `LOW`, `MEDIUM`, `HIGH`, `CUSTOM`)                                                | `"LOW"`         |    ◽     |

### AddOns

| Name                   |                                                      Description                                                      | Value                    | Required |
|:-----------------------|:---------------------------------------------------------------------------------------------------------------------:|--------------------------|:--------:|
| `ci.namespace`         |                                                   Tekton namespaces                                                   | `"kcp-tekton-pipelines"` |    ◽     |
| `ci.tekton.enabled`    |         Tekton is installed or not in cluster (options: `true`,`false`). For installing value will be `true`          | `"true"`                 |    ◽     |
| `loki.url`             |                                    Loki Server Base URl (e.g. "https://loki.com")                                     | `""`                     |    ◽     |
| `loki.username`        |                                                 Loki Server Username                                                  | `""`                     |    ◽     |
| `loki.password`        |                                                 Loki Server Password                                                  | `""`                     |    ◽     |
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
