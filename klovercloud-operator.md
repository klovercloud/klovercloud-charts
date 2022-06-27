# Klovercloud Operator

## TL;DR

```bash
helm repo add klovercloud-charts https://klovercloud.github.io/klovercloud-charts/charts
helm install kc-operator klovercloud-charts/klovercloud-operator -n klovercloud
```


# Klovercloud Operator
## Prerequisites
1. Cert Manager
2. Cluster Issuer
3. helm v3.4.1

## Installation Instructions
To install the operator with the release name  `kc-operator`:


```bash
helm repo add klovercloud-charts https://klovercloud.github.io/klovercloud-charts/charts
helm install kc-operator klovercloud-charts/klovercloud-operator 
 ```
## Uninstallation instructions
```bash
helm delete kc-operator
 ```
## Parameters

### Operator Parameters
| Name                                         |                  Description                   | Value                                         | Required |
|:---------------------------------------------|:----------------------------------------------:|------------------------------------------------|:--------:|
| `operator.image.repository`                  | Klovercloud Operator Public Registry and image | `quay.io/klovercloud/klovercloud-operator-poc` |    ✅     |
| `operator.image.tag`                         |        Operator image version reference        | `latest`                              |    ✅     |
| `operator.namespace`                         |   namespace for the Operator to be deployed    | `"klovercloud"`                                |    ✅     |
| `operator.db.type`                           |         In memory database used for Operator to save its states. Values are `CacheControllerDB`, `BuntDB`                                          | `"CacheControllerDB"`                  |    ◽     |
| `operator.db.includePersistentStorage`       |         If operator storage information needs to be persisted then value will be `true` otherwise `false`                                          | `"false"`                              |    ◽     |

### Cluster Parameters
| Name                                         |                  Description                                     | Value                                          | Required |
|:---------------------------------------------|:----------------------------------------------------------------:|------------------------------------------------|:--------:|
| `cluster.name`                               |                Cluster Name                                      | `"My Cluster"`                                 |    ✅     |
| `cluster.volumes.storageType`                |         Cloud Storage Provisioner Name                           | `"EKS"`                                        |    ✅     |
| `cluster.volumes.storageClass.readWriteMany` |        ReadWriteMany StorageClass Name                           | `"eks-sc-ebs"`                                 |    ✅     |
| `cluster.volumes.storageClass.readWriteOnce` |        ReadWriteMany StorageClass Name                           | `"eks-sc-efs"`                                 |    ✅     |
| `cluster.serviceAccount.name`                |      ServiceAccountName for the Klovercloud Services             | `""`                                           |    ◽     |
| `cluster.volumes.snapshotClass.name`         |           Volume SnapshotClass Name                              | `"ebs-snapclass"`                              |    ✅     |
| `cluster.clusterissuer.name`                 |      Cluster Issuer Name for the Cluster                         | `"letsencrypt-cluster"`                        |    ✅     |
| `cluster.serviceDns.enabled`                 |      Service DNS enabled Or Disabled. If your cluster needs "svc.cluster.local" then set value to `true` otherwise `false`         | `"true"`                          |    ◽     |
| `cluster.securityContext.enabled`            |      Cluster Pod Security Context Enabled or Disabled. Values are `true`, `false`                                                  | `"true"`                          |    ◽     |
| `cluster.notification.url`                   |      Cluster notification Url. If Url is given then Operator will send A POST request notification to the given Url.               | `""`                              |    ◽     |

### Platform Parameters
| Name                                                  |                  Description                              | Value                                          | Required |
|:------------------------------------------------------|:---------------------------------------------------------:|------------------------------------------------|:--------:|
| `platform.namespace`                                  |    Klovercloud Operator Platform Namespace                | `"klovercloud"`                                |    ✅     |
| `platform.db.mongo.password`                          |  Password to access Database for Klovercloud              | `""`                                           |    ◽     |
| `platform.user.companyAdmin.password`                 | Admin Password to Login to Klovercloud Webapp             | `""`                                           |    ✅     |
| `platform.user.companyAdmin.email`                    |    Admin Email for Klovercloud Webapp Login               | `""`                                           |    ✅     |
| `platform.service.domain.wildcard.name`               |          Domain Name to access Webapp                     | `""`                                           |    ✅     |
| `platform.service.domain.wildcard.tlsSecret`          | SSL Certificate to secure connection to Webapp            | `""`                                           |    ✅     |
| `platform.service.servicemesh.domain.wildcard.name`   | Domain Name to access Webapp through Service Mesh         | `""`                                           |    ◽     |


### AddOns
| Name                                         |                  Description                   | Value                                         | Required |
|:---------------------------------------------|:----------------------------------------------:|------------------------------------------------|:--------:|
| `loki.url`                                   |          Loki Server URL                       | `""`                                           |    ◽     |
| `loki.wsurl`                                 |          Loki Websocket URL                    | `""`                                           |    ◽     |
| `loki.username`                              |          Loki Server Username                  | `""`                                           |    ◽     |
| `loki.password`                              |          Loki Server Password                  | `""`                                           |    ◽     |
| `prometheus.url`                             |          Prometheus URL                        | `""`                                           |    ◽     |


## Operator Images Tags

[latest](#) [0.1.5](#)

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
