# Klovercloud Operator

## Prerequisites
1. K8 Cluster
2. Cert Manager
2. Cluster Issuer
3. Helm Version >= v3.4.1
4. Nginx Ingress Controller
5. StorageClass (ReadWriteOnce)

## TL;DR

```
helm repo add klovercloud-charts https://klovercloud.github.io/klovercloud-charts/charts

helm repo update
   
helm install kc-operator --namespace klovercloud klovercloud-charts/klovercloud-operator --version 0.2.5 \
    --set cluster.volumes.storageType="AZURE" \
    --set cluster.volumes.storageClass.readWriteMany=csi-file-storage-efs \
    --set cluster.volumes.storageClass.readWriteOnce=csi-disk \
    --set platform.temporal.host="95.216.152.146:7233" \
    --set platform.temporal.namespace="default" \    
    --set platform.namespace=klovercloud \
    --set platform.user.companyAdmin.password="Abcd1234!" \
    --set platform.user.companyAdmin.email="admin@klovercloud.com" \
    --set platform.user.operatorUser.username="sa@klovercloud.com" \
    --set platform.user.operatorUser.password="KC@saasXD23!" \          
    --set platform.internalServiceRequestThroughIngress="true" \
    --set platform.service.domain.wildcard.tlsSecret="wild-cert-secret" \
    --set platform.service.domain.wildcard.name="cloud.klovercloud.io" \
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
| `0.2.5` |   10/10/24   | 
| `0.2.4` |   10/05/24   | 


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

### Cluster Parameters

| Name                                         |                                                      Description                                                      | Value                   | Required |
|:---------------------------------------------|:---------------------------------------------------------------------------------------------------------------------:|-------------------------|:--------:|
| `cluster.name`                               |                                                     Cluster Name                                                      | `"My Cluster"`          |    ◽     |
| `cluster.volumes.storageType`                |            Cloud Storage Provisioner Name (options: `EKS`, `GCP`,  `AZURE`, `DIGITAL_OCEAN`, `BARE_METAL`)            | `""`                    |    ✅     |
| `cluster.volumes.storageClass.readWriteMany` |                                            ReadWriteMany StorageClass Name                                            | `"eks-sc-ebs"`          |    ✅     |
| `cluster.volumes.storageClass.readWriteOnce` |                                            ReadWriteMany StorageClass Name                                            | `"eks-sc-efs"`          |    ✅     |
| `cluster.serviceAccount.name`                |                                    ServiceAccountName for the Klovercloud Services                                    | `""`                    |    ◽     |
| `cluster.volumes.snapshotClass.name`         |                                               Volume SnapshotClass Name                                               | `"ebs-snapclass"`       |    ◽     |
| `cluster.clusterissuer.name`                 |                                          Cluster Issuer Name for the Cluster                                          | `"letsencrypt-cluster"` |    ◽     |
| `cluster.serviceDns.enabled`                 | Service DNS enabled Or Disabled. If your cluster needs "svc.cluster.local" then set value to `true` otherwise `false` | `"true"`                |    ◽     |
| `cluster.securityContext.enabled`            |                     Cluster Pod Security Context Enabled or Disabled. Values are `true`, `false`                      | `"true"`                |    ◽     |
| `cluster.notification.url`                   |    Cluster notification Url. If Url is given then Operator will send A POST request notification to the given Url.    | `""`                    |    ◽     |

### Platform Parameters

| Name                                                     |                                                                                                                                                                                               Description                                                                                                                                                                                                | Value                                                                | Required |
|:---------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|----------------------------------------------------------------------|:--------:|
| `platform.namespace`                                     |                                                                                                                                                                                 Klovercloud Operator Platform Namespace                                                                                                                                                                                  | `"klovercloud"`                                                      |    ◽     |
| `platform.queue.kafka.server`                            |                                                                                                                                                Kafka Server Address (e.g. kafka-0.kafka.namespace). If not provided, Operator will create a kafka cluster                                                                                                                                                | `""`                                                                 |    ◽     |
| `platform.queue.kafka.port`                              |                                                                                                                                    Kafka Port (e.g. 5323). If `platform.queue.kafka.server`  not provided, Operator will create a kafka cluster  and add defautl port                                                                                                                                    | `""`                                                                 |    ◽     |
| `platform.db.mongo.server`                               |                                                                                                                                               Mongo Server Address (e.g. mongodb-0.mongo.namespace). If not provided, Operator will create a mongo cluster                                                                                                                                               | `""`                                                                 |    ◽     |
| `platform.db.mongo.port`                                 |                                                                                                                                Mongo Server Address (e.g. 21717). If `platform.db.mongo.server` not provided, Operator will create a mongo cluster  and add defautl port                                                                                                                                 | `""`                                                                 |    ◽     |
| `platform.db.mongo.username`                             |                                                                                                                                                                               Username to access Database for Klovercloud                                                                                                                                                                                | `""`                                                                 |    ◽     |
| `platform.db.mongo.password`                             |                                                                                                                                                                               Password to access Database for Klovercloud                                                                                                                                                                                | `""`                                                                 |    ◽     |
| `platform.user.operatorUser.username`                    |                                                                                                                                                                                              Operator User                                                                                                                                                                                               | `""`                                                                 |    ✅     |
| `platform.user.operatorUser.password`                    |                                                                                                                                                                                          Operator User Password                                                                                                                                                                                          | `""`                                                                 |    ✅     |
| `platform.user.companyAdmin.password`                    |                                                                                                                                                                              Admin Password to Login to Klovercloud Webapp                                                                                                                                                                               | `""`                                                                 |    ✅     |
| `platform.user.companyAdmin.email`                       |                                                                                                                                                                                 Admin Email for Klovercloud Webapp Login                                                                                                                                                                                 | `""`                                                                 |    ✅     |
| `platform.service.domain.wildcard.name`                  |                                                                                                                                                                                       Domain Name to access Webapp                                                                                                                                                                                       | `""`                                                                 |    ✅     |
| `platform.internalServiceRequestThroughExternalEndpoint` |                                                                                                                                                           Platform internal service call through External Endpoint (options: `true`, `false`)                                                                                                                                                            | `"true"`                                                             |    ◽     |
| `platform.service.domain.wildcard.tlsSecret`             |                                                                                                                                                     SSL Certificate to secure connection; If value of `platform.ssl.enabled` is `true` then required                                                                                                                                                     | `""`                                                                 |    ◽     |
| `platform.service.tcp.domain.wildcard.name`              |                                                                                                                                                                                Domain Name to connect using TCP protocol                                                                                                                                                                                 | `""`                                                                 |    ◽     |
| `platform.service.tcp.domain.wildcard.tlsSecret`         |                                                                                                                                                                               SSL Certificate to secure connection to TCP                                                                                                                                                                                | `""`                                                                 |    ◽     |
| `platform.service.servicemesh.domain.wildcard.name`      |                                                                                                                                                                            Domain Name to access Webapp through Service Mesh                                                                                                                                                                             | `""`                                                                 |    ◽     |
| `platform.service.webapp.domain`                         |                                                                                                                              Dedicated domain for Klovercloud Console. If not given then the value will be the value of `console.${platform.service.domain.wildcard.name}`                                                                                                                               | `""`                                                                 |    ◽     |
| `platform.service.facade.domain`                         |                                                                                                                               Dedicated domain for Klovercloud Api Server. If not given then the value will be the value of `api.${platform.service.domain.wildcard.name}`                                                                                                                               | `""`                                                                 |    ◽     |
| `platform.service.multiClusterConsoleGateway.domain`     |                                                                                                                          Dedicated domain for Klovercloud Gateway Service. If not given then the value will be the value of `gateway.${platform.service.domain.wildcard.name}`                                                                                                                           | `""`                                                                 |    ◽     |
| `platform.service.listener.domain`                       |                                                                                                                         Dedicated domain for Klovercloud Listener Service. If not given then the value will be the value of `listener.${platform.service.domain.wildcard.name}`                                                                                                                          | `""`                                                                 |    ◽     |
| `platform.service.proxyServer.domain`                    |                                                                                                                             Dedicated domain for Klovercloud Proxy Server. If not given then the value will be the value of `proxy.${platform.service.domain.wildcard.name}`                                                                                                                             | `""`                                                                 |    ◽     |
| `platform.service.webapp.disabledFeatures`               | Disabled Services (options: `queue,object-storage,marketplace,billing,my-plans,support,ticket,registration,cluster,create_cluster,create_onboard_cluster,cloud-provider,k8s,helm-apps,system-setting,sso,namespace,application,database,cache,queue,object-storage,kcr,marketplace,load-balancer,external-endpoint,dns,support,ticket,billing,activity-log,manage-account-setting,manage-build-setting`) | `"queue,object-storage,marketplace,billing,my-plans,support,ticket"` |    ◽     |
| `platform.service.webapp.istioEnabled`                   |                                                                                                                                                                     Istio Enabled for Webapp and Cluster (options: `true`, `false`)                                                                                                                                                                      | `"true`                                                              |    ◽     |
| `platform.paymentGateway.mode`                           |                                                                                                                                                    Payment Gateway Mode (options: `PRODUCTION`, `TEST`) Required if billing feature is not disabled.                                                                                                                                                     | `"PRODUCTION`                                                        |    ◽     |
| `platform.paymentGateway.sslCommerz.storeId`             |                                                                                                                                                                                   Payment Gateway SSL Commerz Store ID                                                                                                                                                                                   | `""`                                                                 |    ◽     |
| `platform.paymentGateway.sslCommerz.storePassword`       |                                                                                                                                                                               Payment Gateway SSL Commerz Store Password.                                                                                                                                                                                | `""`                                                                 |    ◽     |
| `platform.paymentGateway.stripe.publicKey`               |                                                                                                                                                                                    Payment Gateway Stripe Public Key.                                                                                                                                                                                    | `""`                                                                 |    ◽     |
| `platform.paymentGateway.stripe.secretKey`               |                                                                                                                                                                                      Payment Gateway StriSecret Key                                                                                                                                                                                      | `""`                                                                 |    ◽     |
| `platform.temporal.host`                                 |                                                                                                                                                                                              Temporal Host                                                                                                                                                                                               | `""`                                                                 |    ✅     |
| `platform.temporal.namespace`                            |                                                                                                                                                                                            Temporal Namespace                                                                                                                                                                                            | `""`                                                                 |    ✅     |
| `platform.argocd.url`                                    |                                                                                                                                                                                                ArgoCD URL                                                                                                                                                                                                | `""`                                                                 |    ◽     |
| `platform.user.adminUser.enabled`                        |                                                                                                                            Is Admin User for each company verfied by default  (options: `true`, `false`). If `false` admin user needs to be verfied through email verfication                                                                                                                            | `"false"`                                                            |    ◽     |
| `platform.smtp.server`                                   |                                                                                                                                                                                               SMTP Server                                                                                                                                                                                                | `""`                                                                 |    ◽     |
| `platform.smtp.port`                                     |                                                                                                                                                                                             SMTP Server Port                                                                                                                                                                                             | `""`                                                                 |    ◽     |
| `platform.smtp.username`                                 |                                                                                                                                                                                           SMTP Server Username                                                                                                                                                                                           | `""`                                                                 |    ◽     |
| `platform.smtp.password`                                 |                                                                                                                                                                                           SMTP Server Password                                                                                                                                                                                           | `""`                                                                 |    ◽     |
| `platform.smtp.mail.address`                             |                                                                                                                                                                                         SMTP Server Mail Address                                                                                                                                                                                         | `""`                                                                 |    ◽     |
| `platform.smtp.mail.Template`                            |                                                                                                                                                                                        SMTP Server Mail Template                                                                                                                                                                                         | `""`                                                                 |    ◽     |
| `platform.smtp.mail.noReplyMessage`                      |                                                                                                                                                                                       SMTP Server No Message Reply                                                                                                                                                                                       | `""`                                                                 |    ◽     |
| `platform.ticketing.s3BucketName`                        |                                                                                                                                                      Aws Bucket name for ticketing to store ticketing files; Required if ticketing service enabled                                                                                                                                                       | `""`                                                                 |    ◽     |

### AddOns

| Name                    |                                                          Description                                                           | Value | Required |
|:------------------------|:------------------------------------------------------------------------------------------------------------------------------:|-------|:--------:|
| `ci.tekton.enabled`     |                                                         Tekton Enabled                                                         | `""`  |    ◽     |
| `aws.accessKey`         |                         Aws Access key to store ticketing service files; Required if Ticketing Enabled                         | `""`  |    ◽     |
| `aws.secretKey`         |                         Aws Secret key to store ticketing service files, Required if Ticketing Enabled                         | `""`  |    ◽     |
| `harbor.adminAuthToken` | Harbor Admin Auth Token. `Basic base64Encoded<adminUsername:adminPassword>`; Required if Container Registry Service is Enabled | `""`  |    ◽     |
| `harbor.adminPassword`  |                            Harbor Admin Password, Required if Container Registry Service is Enabled                            | `""`  |    ◽     |
| `harbor.adminUsername`  |                            Harbor Admin Username, Required if Container Registry Service is Enabled                            | `""`  |    ◽     |
| `harbor.url`            |                                 Harbor URL, Required if Container Registry Service is Enabled                                  | `""`  |    ◽     |

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
