# Klovercloud Operator

## Prerequisites
1. K8 Cluster
2. Cert Manager
2. Cluster Issuer
3. Helm Version >= v3.4.1
4. Nginx Ingress Controller
5. StorageClass (ReadWriteOnce)

## TL;DR

```bash
helm repo add klovercloud-charts https://klovercloud.github.io/klovercloud-charts/charts

helm repo update

helm install kc-operator --namespace klovercloud klovercloud-charts/klovercloud-operator --version 0.2.1 \
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

To install the operator with the release name  `kc-operator`:

## Uninstallation instructions
```bash
helm delete kc-operator
 ```

# Releases

## Operator chart version

| version | Release Date |
|:--------|:------------:|
| `0.2.1` |   10/10/24   | 


## Operator Image Tags

| tag    | Release Date |
|:-------|:------------:|
| `v2.0` |   10/10/24   | 


## Parameters

### Operator Parameters

| Name                                   |                                            Description                                            | Value                                          | Required |
|:---------------------------------------|:-------------------------------------------------------------------------------------------------:|------------------------------------------------|:--------:|
| `operator.image.repository`            |                          Klovercloud Operator Public Registry and image                           | `quay.io/klovercloud/klovercloud-operator-poc` |    ✅     |
| `operator.image.tag`                   |                                 Operator image version reference                                  | `latest`                                       |    ✅     |
| `operator.namespace`                   |                             namespace for the Operator to be deployed                             | `"klovercloud"`                                |    ✅     |
| `operator.db.type`                     | In memory database used for Operator to save its states. Values are `CacheControllerDB`, `BuntDB` | `"CacheControllerDB"`                          |    ◽     |
| `operator.db.includePersistentStorage` | If operator storage information needs to be persisted then value will be `true` otherwise `false` | `"false"`                                      |    ◽     |

### Cluster Parameters

| Name                                         |                                                      Description                                                      | Value                   | Required |
|:---------------------------------------------|:---------------------------------------------------------------------------------------------------------------------:|-------------------------|:--------:|
| `cluster.name`                               |                                                     Cluster Name                                                      | `"My Cluster"`          |    ✅     |
| `cluster.volumes.storageType`                |                                            Cloud Storage Provisioner Name                                             | `"EKS"`                 |    ✅     |
| `cluster.volumes.storageClass.readWriteMany` |                                            ReadWriteMany StorageClass Name                                            | `"eks-sc-ebs"`          |    ✅     |
| `cluster.volumes.storageClass.readWriteOnce` |                                            ReadWriteMany StorageClass Name                                            | `"eks-sc-efs"`          |    ✅     |
| `cluster.serviceAccount.name`                |                                    ServiceAccountName for the Klovercloud Services                                    | `""`                    |    ◽     |
| `cluster.volumes.snapshotClass.name`         |                                               Volume SnapshotClass Name                                               | `"ebs-snapclass"`       |    ✅     |
| `cluster.clusterissuer.name`                 |                                          Cluster Issuer Name for the Cluster                                          | `"letsencrypt-cluster"` |    ✅     |
| `cluster.serviceDns.enabled`                 | Service DNS enabled Or Disabled. If your cluster needs "svc.cluster.local" then set value to `true` otherwise `false` | `"true"`                |    ◽     |
| `cluster.securityContext.enabled`            |                     Cluster Pod Security Context Enabled or Disabled. Values are `true`, `false`                      | `"true"`                |    ◽     |
| `cluster.notification.url`                   |    Cluster notification Url. If Url is given then Operator will send A POST request notification to the given Url.    | `""`                    |    ◽     |

### Platform Parameters

| Name                                                |                                                                      Description                                                                      | Value           | Required |
|:----------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------:|-----------------|:--------:|
| `platform.namespace`                                |                                                        Klovercloud Operator Platform Namespace                                                        | `"klovercloud"` |    ✅     |
| `platform.queue.kafka.server`                       |                      Kafka Server Address (e.g. kafka-0.kafka.namespace). If not provided, Operator will create a kafka cluster                       | `""`            |    ◽     |
| `platform.queue.kafka.port`                         |          Kafka Port (e.g. 5323). If `platform.queue.kafka.server`  not provided, Operator will create a kafka cluster  and add defautl port           | `""`            |    ◽     |
| `platform.db.mongo.server`                          |                     Mongo Server Address (e.g. mongodb-0.mongo.namespace). If not provided, Operator will create a mongo cluster                      | `""`            |    ◽     |
| `platform.db.mongo.port`                            |       Mongo Server Address (e.g. 21717). If `platform.db.mongo.server` not provided, Operator will create a mongo cluster  and add defautl port       | `""`            |    ◽     |
| `platform.db.mongo.username`                        |                                                      Username to access Database for Klovercloud                                                      | `""`            |    ◽     |
| `platform.db.mongo.password`                        |                                                      Password to access Database for Klovercloud                                                      | `""`            |    ◽     |
| `platform.user.companyAdmin.password`               |                                                     Admin Password to Login to Klovercloud Webapp                                                     | `""`            |    ✅     |
| `platform.user.companyAdmin.email`                  |                                                       Admin Email for Klovercloud Webapp Login                                                        | `""`            |    ✅     |
| `platform.service.domain.wildcard.name`             |                                                             Domain Name to access Webapp                                                              | `""`            |    ✅     |
| `platform.service.domain.wildcard.tlsSecret`        |                                                    SSL Certificate to secure connection to Webapp                                                     | `""`            |    ✅     |
| `platform.service.tcp.domain.wildcard.name`         |                                                       Domain Name to connect using TCP protocol                                                       | `""`            |    ◽     |
| `platform.service.tcp.domain.wildcard.tlsSecret`    |                                                      SSL Certificate to secure connection to TCP                                                      | `""`            |    ◽     |
| `platform.service.servicemesh.domain.wildcard.name` |                                                   Domain Name to access Webapp through Service Mesh                                                   | `""`            |    ◽     |
| `platform.service.webapp.domain`                    |          Dedicated domain for Klovercloud Console. If not given then the value will be the value of `$platform.service.domain.wildcard.name`          | `""`            |    ◽     |
| `platform.service.facade.domain`                    |      Dedicated domain for Klovercloud Api Server. If not given then the value will be the value of `api.$platform.service.domain.wildcard.name`       | `""`            |    ◽     |
| `platform.service.listener.domain`                  | Dedicated domain for Klovercloud Listener Service. If not given then the value will be the value of `listener.$platform.service.domain.wildcard.name` | `""`            |    ◽     |

### AddOns
| Name                                         |                  Description                   | Value                                         | Required |
|:---------------------------------------------|:----------------------------------------------:|------------------------------------------------|:--------:|
| `loki.url`                                   |          Loki Server URL                       | `""`                                           |    ◽     |
| `loki.wsurl`                                 |          Loki Websocket URL                    | `""`                                           |    ◽     |
| `loki.username`                              |          Loki Server Username                  | `""`                                           |    ◽     |
| `loki.password`                              |          Loki Server Password                  | `""`                                           |    ◽     |
| `prometheus.url`                             |          Prometheus URL                        | `""`                                           |    ◽     |


## Notification Webhook
To receive event notification from the operator, set cluster.notification.url to expect the following post request from the operator:
```bash
{
     "clusterConfig" : {
         "apiServerEndpoint" : "",
         "controlPlaneEndpoint":  ""
        },
    "data" : {
        "createdAt" : "",
        "currentStep" : 1,
        "extras" : null,
        "log" : "",
        "totalStep" : 8
    },
    "status" : ""
}
```
