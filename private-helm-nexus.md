## How to create Private helm repo in Nexus Artifactory.

![image](https://user-images.githubusercontent.com/83489863/234300857-e0d7d6ab-e7b5-40ca-ab77-7afb2e9553e3.png)

```
Name: helm-jnrlabs
Online: checked
Blob store: default
Strict content type Validation: checked
Deployment Policy: Disable redeploy
```

```
root@dev-clus01:~# helm repo add helm-jnrlabs http://192.168.9.131:8081/repository/helm-jnrlabs/ --username admin --password password
"helm-jnrlabs" has been added to your repositories

root@dev-clus01:~# helm repo ls
NAME            URL
my-repo         https://charts.bitnami.com/bitnami
helm-jnrlabs    http://192.168.9.131:8081/repository/helm-jnrlabs/

root@dev-clus01:~# helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "helm-jnrlabs" chart repository

```
## Creating new charts.
```
root@dev-clus01:~# helm create hello-world
Creating hello-world

touch hello-world/templates/mychart-configmap.yaml
cat hello-world/templates/mychart-configmap.yaml

apiVerion: v1
kind: ConfigMap
metadata:
  name: mychart-configmap
data:
  myvalue: "Sample config map"


helm package hello-world
curl -u admin:password  http://192.168.9.131:8081/repository/helm-jnrlabs/ --upload-file hello-world.tgz
```
