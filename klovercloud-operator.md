# Klovercloud Operator

## TL;DR

```bash
$ helm repo add klovercloud-charts https://klovercloud.github.io/klovercloud-charts/charts
$ helm install kc-operator klovercloud-charts/klovercloud-operator
```


## Klovercloud Operator

```
helm install kc-operator klovercloud-charts/klovercloud-operator --dry-run

helm install kc-operator klovercloud-charts/klovercloud-operator \ 
> --set operator.image.repository=quay.io/klovercloud/klovercloud-operator-multicluster \
> --set operator.image.tag=0.1.0 \
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
> --set platform.service.domain.wildcard.name=klovercloud.io \
> --set platform.service.domain.wildcard.tlsSecret=secret-name --dry-run
```