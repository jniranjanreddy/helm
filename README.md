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

[root@minikube01 ~]# helm search repo stable/mysql
NAME                    CHART VERSION   APP VERSION     DESCRIPTION
stable/mysql            1.6.9           5.7.30          DEPRECATED - Fast, reliable, scalable, and easy...
stable/mysqldump        2.6.2           2.4.1           DEPRECATED! - A Helm chart to help backup MySQL...
```


```
Installing MySql through helm..

[root@minikube01 ~]# helm install stable/mysql --generate-name
WARNING: This chart is deprecated
NAME: mysql-1622100113
LAST DEPLOYED: Thu May 27 03:22:04 2021
NAMESPACE: kubernetes-dashboard
STATUS: deployed
REVISION: 1
NOTES:
MySQL can be accessed via port 3306 on the following DNS name from within your cluster:
mysql-1622100113.kubernetes-dashboard.svc.cluster.local

To get your root password run:

    MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace kubernetes-dashboard mysql-1622100113 -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)

To connect to your database:

1. Run an Ubuntu pod that you can use as a client:

    kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il

2. Install the mysql client:

    $ apt-get update && apt-get install mysql-client -y

3. Connect using the mysql cli, then provide your password:
    $ mysql -h mysql-1622100113 -p

To connect to your database directly from outside the K8s cluster:
    MYSQL_HOST=127.0.0.1
    MYSQL_PORT=3306

    # Execute the following command to route the connection:
    kubectl port-forward svc/mysql-1622100113 3306

    mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}

Lets us install one more from app through Helm..

helm install myairflow stable/airflow
[root@minikube01 ~]# helm install hello-airflow stable/airflow
WARNING: This chart is deprecated
NAME: hello-airflow
LAST DEPLOYED: Thu May 27 03:39:16 2021
NAMESPACE: kubernetes-dashboard
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
!WARNING! !WARNING! !WARNING! !WARNING! !WARNING!

This chart has MOVED to a new repository!

https://github.com/airflow-helm/charts/tree/main/charts/airflow

!WARNING! !WARNING! !WARNING! !WARNING! !WARNING!

--------------------

Congratulations. You have just deployed Apache Airflow!

1. Get the Airflow Service URL by running these commands:
   export POD_NAME=$(kubectl get pods --namespace kubernetes-dashboard -l "component=web,app=airflow" -o jsonpath="{.items[0].metadata.name}")
   echo http://127.0.0.1:8080
   kubectl port-forward --namespace kubernetes-dashboard $POD_NAME 8080:8080

2. Open Airflow in your web browser



```
