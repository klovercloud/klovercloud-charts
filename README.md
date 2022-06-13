# Klovercloud Charts
```
helm repo add klovercloud-charts https://klovercloud.github.io/klovercloud-charts/charts
```

## Klovercloud Operator

```
helm install kc-operator klovercloud-charts/klovercloud-operator --dry-run

helm install kc-operator klovercloud-charts/klovercloud-operator \ 
> --set operator.image.repository=quay.io/klovercloud/klovercloud-operator \
> --set operator.image.tag=multicluster-v0.1.0 \
> --set operator.namespace=klovercloud \
> --set platform.namespace=klovercloud \
> --set platform.user.companyAdmin.password=klovercloud \
> --set platform.user.companyAdmin.email=admin@klovercloud.com \
> --set cluster.volumes.storageType=EKS \
> --set cluster.volumes.storageClass.readWriteMany=efs-sc \
> --set cluster.volumes.storageClass.readWriteOnce=ebs-sc \
> --set cluster.serviceAccount.name=avc \
> --set cluster.volumes.snapshotClass.name=avc \
> --set cluster.clusterissuer.name=avc \
> --set platform.service.wildCardDomain=klovercloud.io \
> --set platform.service.wildCardTlsSecret=secret-name --dry-run
```
