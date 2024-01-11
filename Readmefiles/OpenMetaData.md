## Source: https://docs.open-metadata.org/v1.0.0/deployment/kubernetes/onprem
## Source: https://github.com/open-metadata/openmetadata-helm-charts

## Create Secrets
```
kubectl create secret generic mysql-secrets --from-literal=openmetadata-mysql-password=openmetadata_password
kubectl create secret generic airflow-secrets --from-literal=openmetadata-airflow-password=admin
kubectl create secret generic airflow-mysql-secrets --from-literal=airflow-mysql-password=airflow_pass
```
## Add Metadata helm Repo
```
helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner

helm repo add open-metadata https://helm.open-metadata.org/
helm install openmetadata-dependencies open-metadata/openmetadata-dependencies
```




## Accessing URL
```
kubectl port-forward deployment/openmetadata 8585:8585
```
