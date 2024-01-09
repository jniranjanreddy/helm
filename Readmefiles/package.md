## How to package and Push.
```
root@minikube01 /myworkspace/helm (main) # helm package test-01/
Successfully packaged chart and saved it to: /myworkspace/helm/test-01-0.1.0.tgz


curl -u admin:password http://192.168.9.19:8081/repository/helm/ --upload-file cm-chart-0.1.0.tgz


root@minikube01 /myworkspace/helm (main) # helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "helm-stack01" chart repository
...Successfully got an update from the "localstack" chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈Happy Helming!⎈


root@minikube01 /myworkspace/helm (main) # helm search repo helm-stack01
NAME                    CHART VERSION   APP VERSION     DESCRIPTION
helm-stack01/cm-chart   0.1.0           1.16.0          A Helm chart for Kubernetes


```
