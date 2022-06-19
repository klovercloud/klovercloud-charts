# Klovercloud Operator

## TL;DR

```bash
$ helm repo add klovercloud-charts https://klovercloud.github.io/klovercloud-charts/charts
$ helm install kc-operator klovercloud-charts/klovercloud-operator
```


## Klovercloud Operator

#Prerequisites

#Installation Instructions
To install the operator with the release name  `kc-operator`:


```bash
$ helm repo add klovercloud-charts https://klovercloud.github.io/klovercloud-charts/charts
$ helm install kc-operator klovercloud-charts/klovercloud-operator 
 ```
#Uninstallation instructions
```bash
$ helm delete kc-operator
 ```
#Parameters

| Name                                         |                   Description                   | Value    | Required |
|:---------------------------------------------|:-----------------------------------------------:|----------|:--------:|
| `operator.image.repository`                  |                                                 | ``       |    ◽     |
| `operator.image.tag`                         | Global Docker registry secret names as an array | `Latest` |    ◽     |
| `operator.namespace`                         |                                                 | `""`     |    ✅     |
| `platform.namespace`                         |                                                 | `""`     |    ✅     |
| `platform.user.companyAdmin.password`        |                                                 | `""`     |    ✅     |
| `platform.user.companyAdmin.email`           |                                                 | `""`     |    ✅     |
| `cluster.volumes.storageType`                |                                                 | `""`     |    ✅     |
| `cluster.volumes.storageClass.readWriteMan`  |                                                 | `""`     |    ✅     |
| `cluster.volumes.storageClass.readWriteOnce` |                                                 | `""`     |    ✅     |
| `cluster.serviceAccount.name`                |                                                 | `""`     |    ✅     |
| `cluster.volumes.snapshotClass.name`         |                                                 | `""`     |    ✅     |
| `cluster.clusterissuer.name`                 |                                                 | `""`     |    ✅     |
| `platform.service.domain.wildcard.name`      |                                                 | `""`     |    ✅     |
| `platform.service.domain.wildcard.tlsSecret` |                                                 | `""`     |    ✅     |
| `tlsSecret`                                  |                                                 | `""`     |    ◽️    |


> 
