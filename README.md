# helm
```
How to install helm.
Download binary, make executable and copy helm to /usr/local/bin/   # Thats all

wget https://get.helm.sh/helm-v3.5.0-linux-amd64.tar.gz
tar -zxvf helm-v3.5.0-linux-amd64.tar.gz
chmod +x linux-amd64/helm 
cp linux-amd64/helm  /usr/local/bin/

[root@minikube01 tmp]# helm version
version.BuildInfo{Version:"v3.5.0", GitCommit:"32c22239423b3b4ba6706d450bd044baffdcf9e6", GitTreeState:"clean", GoVersion:"go1.15.6"}

[root@minikube01 ~]# helm repo list
Error: no repositories to show

[root@minikube01 /]#  helm repo add stable https://charts.helm.sh/stable
"stable" has been added to your repositories

[root@minikube01 ~]# helm repo list
NAME    URL
stable  https://charts.helm.sh/stable

[root@minikube01 ~]# helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈Happy Helming!⎈

```
