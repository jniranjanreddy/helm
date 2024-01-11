
```
helm repo add helm-stack01  http://192.168.9.19:8081/repository/helm --username admin --password password


helm search repo stable/mysql

```
## Installations...
```
helm install stable/mysql --generate-name
```