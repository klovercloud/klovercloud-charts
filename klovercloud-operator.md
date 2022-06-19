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
| Name                                         |                  Description                   | Value                                          | Required |
|:---------------------------------------------|:----------------------------------------------:|------------------------------------------------|:--------:|
| `operator.image.repository`                  | Klovercloud Operator Public Registry and image | `quay.io/klovercloud/klovercloud-operator-poc` |    ✅     |
| `operator.image.tag`                         |        Operator image version reference        | `latest`, `0.1.2`                              |    ✅     |
| `operator.namespace`                         |   namespace for the Operator to be deployed    | `"klovercloud"`                                |    ✅     |
| `platform.namespace`                         |    Klovercloud Operator Platform Namespace     | `"klovercloud"`                                |    ✅     |
| `platform.user.companyAdmin.password`        | Admin Password to Login to Klovercloud Webapp  | `"password"`                                   |    ✅     |
| `platform.user.companyAdmin.email`           |    Admin Email for Klovercloud Webapp Login    | `"admin@klovercloud.com"`                      |    ✅     |
| `db.mongo.password`                          |  Password to access Database for Klovercloud   | `"password"`                                   |    ◽     |
| `cluster.volumes.storageType`                |         Cloud Storage Provisioner Name         | `"EKS"`                                        |    ✅     |
| `cluster.volumes.storageClass.readWriteMany` |        ReadWriteMany StorageClass Name         | `"eks-sc-ebs"`                                 |    ✅     |
| `cluster.volumes.storageClass.readWriteOnce` |        ReadWriteMany StorageClass Name         | `"eks-sc-efs"`                                 |    ✅     |
| `cluster.serviceAccount.name`                |      ServiceAccountName for the Operator       | `"klovercloud-operator-sa"`                    |    ✅     |
| `cluster.volumes.snapshotClass.name`         |           Volume SnapshotClass Name            | `"ebs-snapclass"`                              |    ✅     |
| `cluster.clusterissuer.name`                 |      Cluster Issuer Name for the Cluster       | `"letsencrypt-cluster"`                        |    ✅     |
| `platform.service.domain.wildcard.name`      |          Domain Name to access Webapp          | `"eks.klovercloud.io"`                         |    ✅     |
| `platform.service.domain.wildcard.tlsSecret` | SSL Certificate to secure connection to Webapp | `"wild-cert-secret"`                           |    ✅     |
| `cluster.name`                               |                EKS Cluster Name                | `"EKS-CLUSTER-NAME"`                           |    ✅     |
| `cluster.notification.url`                   |          Webhook URL for Notification          | `"notifications.klovercloud.io"`               |    ◽     |

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
