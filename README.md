# helm
Important URLs to know for extensive use of helm.
1. https://helm.sh/docs/
2. https://pkg.go.dev/text/template
3. http://masterminds.github.io/sprig/strings.html

```
Helm 2.x 
wget https://get.helm.sh/helm-v2.16.11-linux-amd64.tar.gz
tar -zxvf helm-v2.16.11-linux-amd64.tar.gz
mv linux-amd64/helm /usr/local/bin/helm

[root@k8s-helm ~]# helm init --stable-repo-url=https://charts.helm.sh/stable
$HELM_HOME has been configured at /root/.helm.
Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.

[root@minikube01 tmp]# helm version
Client: &version.Version{SemVer:"v2.16.11", GitCommit:"73b28bab84490d18ab1b71489a574ee18e229eea", GitTreeState:"clean"}

#If you see any issues with tiller, then in one terminal execute the command and use helm on other terminal
[root@minikube01 myworkspace]#  kubectl -n kube-system port-forward svc/tiller-deploy 44134:44134
Forwarding from 127.0.0.1:44134 -> 44134
Forwarding from [::1]:44134 -> 44134
Handling connection for 44134

# if you see below error then install socat package.
[root@k8s-helm ~]# helm ls
E0618 01:33:44.063211    5152 portforward.go:400] an error occurred forwarding 44209 -> 44134: error forwarding port 44134 to pod 49855573bfba481f4acd468d95a741190127432f0f8f06aab0d0560471178c57, uid : unable to do port forwarding: socat not found

yum install -y socat  # It should work now.

```

```
How to install helm.
Download binary, make executable and copy helm to /usr/local/bin/   # Thats all

wget https://get.helm.sh/helm-v3.5.0-linux-amd64.tar.gz  OR wget https://get.helm.sh/helm-v3.12.0-linux-amd64.tar.gz
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

```
How to list existing  charts..
[root@minikube01 ~]# helm ls
NAME                    NAMESPACE               REVISION        UPDATED                                 STATUS          CHART           APP VERSION
hello-airflow           kubernetes-dashboard    1               2021-05-27 03:39:16.985492418 -0400 EDT deployed        airflow-7.13.3  1.10.12
myairflow               kubernetes-dashboard    1               2021-05-27 03:31:36.223566744 -0400 EDT deployed        airflow-7.13.3  1.10.12
mysql-1622100113        kubernetes-dashboard    1               2021-05-27 03:22:04.693545735 -0400 EDT deployed        mysql-1.6.9     5.7.30
```

```
How to uninstall Charts..

[root@minikube01 ~]# helm uninstall hello-airflow
release "hello-airflow" uninstalled
```
```
How to create charts..

[root@minikube01 ~]# helm create mychart
Creating mychart

[root@minikube01 ~]# ls
mychart

[root@minikube01 ~]# tree mychart
mychart
├── charts
├── Chart.yaml
├── templates
│   ├── deployment.yaml
│   ├── _helpers.tpl
│   ├── hpa.yaml
│   ├── ingress.yaml
│   ├── NOTES.txt
│   ├── serviceaccount.yaml
│   ├── service.yaml
│   └── tests
│       └── test-connection.yaml
└── values.yaml

3 directories, 10 files

Example:1
Deploy configmap with chart.
Create Configmap file in templates folder, delete all other files in it.

[root@k8s-mas01 helm]# cat mychart/templates/mychart-configmap
apiVerion: v1
kind: ConfigMap
metadata:
  name: mychart-configmap
data:
  myvalue: "Sample config map"

[root@k8s-mas01 helm]# helm install mychart-configmap  ./mychart
NAME: mychart-configmap
LAST DEPLOYED: Sat May 29 18:47:21 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None

we can test through kubectl describe command.
[root@k8s-mas01 helm]# kubectl describe configmaps mychart-configmap
Name:         mychart-configmap
Namespace:    default
Labels:       app.kubernetes.io/managed-by=Helm
Annotations:  meta.helm.sh/release-name: mychart-configmap
              meta.helm.sh/release-namespace: default

Data
====
myvalue:
----
Sample config map
Events:  <none>
[root@k8s-mas01 helm]#

```
```
[root@k8s-mas01 helm]# helm ls
NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
mychart-configmap       default         1               2021-05-29 18:47:21.764497653 -0400 EDT deployed        mychart-0.1.0   1.16.0
[root@k8s-mas01 helm]#
```
```
[root@k8s-mas01 helm]# helm uninstall mychart-configmap
release "mychart-configmap" uninstalled

[root@k8s-mas01 helm]# helm ls
NAME    NAMESPACE       REVISION        UPDATED STATUS  CHART   APP VERSION
[root@k8s-mas01 helm]#

```
```
Example-2
[root@k8s-mas01 helm]# cat mychart/templates/mychart-configmap
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Sample config map"
  
[root@k8s-mas01 helm]# helm install releasename-01  ./mychart
NAME: releasename-01
LAST DEPLOYED: Sat May 29 19:06:51 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None

[root@k8s-mas01 helm]# helm ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
releasename-01  default         1               2021-05-29 19:06:51.515362781 -0400 EDT deployed        mychart-0.1.0   1.16.0
[root@k8s-mas01 helm]#

[root@k8s-mas01 helm]# helm get manifest releasename-01
---
# Source: mychart/templates/mychart-configmap
apiVersion: v1
kind: ConfigMap
metadata:
  name: releasename-01-configmap
data:
  myvalue: "Sample config map"
  
```
```
[root@k8s-mas01 helm]# cat mychart/templates/mychart-configmap
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Sample config map"
[root@k8s-mas01 helm]# kubectl describe configmap releasename-01-configmap
Name:         releasename-01-configmap
Namespace:    default
Labels:       app.kubernetes.io/managed-by=Helm
Annotations:  meta.helm.sh/release-name: releasename-01
              meta.helm.sh/release-namespace: default

Data
====
myvalue:
----
Sample config map
Events:  <none>
```
```
How to do Dry Run
[root@k8s-mas01 helm]# helm install --debug --dry-run firstDryRun ./mychart/
install.go:173: [debug] Original chart version: ""
install.go:190: [debug] CHART PATH: /myworkspace/helm/mychart

NAME: firstDryRun
LAST DEPLOYED: Sat May 29 19:25:12 2021
NAMESPACE: default
STATUS: pending-install
REVISION: 1
TEST SUITE: None
USER-SUPPLIED VALUES:
{}

COMPUTED VALUES:
costCode: CC98112

HOOKS:
MANIFEST:
---
# Source: mychart/templates/mychart-configmap
apiVersion: v1
kind: ConfigMap
metadata:
  name: firstDryRun-configmap
data:
  myvalue: "Sample config map"
  costCode: CC98112

[root@k8s-mas01 helm]#
```
```
Dynamic values or overriding values in the config file
[root@k8s-mas01 helm]# helm install --dry-run --debug --set costCode=CC000 valuesetexample ./mychart/
install.go:173: [debug] Original chart version: ""
install.go:190: [debug] CHART PATH: /myworkspace/helm/mychart

NAME: valuesetexample
LAST DEPLOYED: Sat May 29 19:50:10 2021
NAMESPACE: default
STATUS: pending-install
REVISION: 1
TEST SUITE: None
USER-SUPPLIED VALUES:
costCode: CC000

COMPUTED VALUES:
costCode: CC000

HOOKS:
MANIFEST:
---
# Source: mychart/templates/mychart-configmap
apiVersion: v1
kind: ConfigMap
metadata:
  name: valuesetexample-configmap
data:
  myvalue: "Sample config map"
  costCode: CC000

[root@k8s-mas01 helm]#

```
```
helm repo add my-repo https://charts.bitnami.com/bitnami
helm install my-release my-repo/postgresql-ha

root@minikube01# helm search repo bitnami/postgresql
NAME                    CHART VERSION   APP VERSION     DESCRIPTION
bitnami/postgresql      12.2.8          15.2.0          PostgreSQL (Postgres) is an open source object-...
bitnami/postgresql-ha   11.3.0          15.2.0          This PostgreSQL cluster solution includes the P...
root@minikube01#

root@minikube01# helm pull bitnami/postgresql-ha
```

```
Type-1
export POSTGRES_PASSWORD=postgrespass
export REPMGR_PASSWORD=repmgrpass
helm install postgres-test bitnami/postgresql-ha --set postgresql.password=$POSTGRES_PASSWORD --set postgresql.repmgrPassword=$REPMGR_PASSWORD
```
# bitnami/postgresql-ha  Nodeport
```
helm install postgres-ha my-repo/postgresql-ha --set postgresql.password=$POSTGRES_PASSWORD --set postgresql.repmgrPassword=$REPMGR_PASSWORD --set service.type=NodePort
```
```
Type-one output
NAME: my-release
LAST DEPLOYED: Mon Apr 24 21:06:31 2023
NAMESPACE: development
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: postgresql-ha
CHART VERSION: 11.4.1
APP VERSION: 15.2.0
** Please be patient while the chart is being deployed **
PostgreSQL can be accessed through Pgpool via port 5432 on the following DNS name from within your cluster:

    my-release-postgresql-ha-pgpool.development.svc.cluster.local

Pgpool acts as a load balancer for PostgreSQL and forward read/write connections to the primary node while read-only connections are forwarded to standby nodes.

To get the password for "postgres" run:

    export POSTGRES_PASSWORD=$(kubectl get secret --namespace development my-release-postgresql-ha-postgresql -o jsonpath="{.data.password}" | base64 -d)

To get the password for "repmgr" run:

    export REPMGR_PASSWORD=$(kubectl get secret --namespace development my-release-postgresql-ha-postgresql -o jsonpath="{.data.repmgr-password}" | base64 -d)

To connect to your database run the following command:

    kubectl run my-release-postgresql-ha-client --rm --tty -i --restart='Never' --namespace development --image docker.io/bitnami/postgresql-repmgr:15.2.0-debian-11-r23 --env="PGPASSWORD=$POSTGRES_PASSWORD"  \
        --command -- psql -h my-release-postgresql-ha-pgpool -p 5432 -U postgres -d postgres

To connect to your database from outside the cluster execute the following commands:

    kubectl port-forward --namespace development svc/my-release-postgresql-ha-pgpool 5432:5432 &
    psql -h 127.0.0.1 -p 5432 -U postgres -d postgres



Tyte-2
helm install --set service.type=ClusterIP --set  postgresql.password=postgres --set pgpool.replicaCount=1 --set postgresql.repmgrReconnectAttempts=1 --set postgresql.repmgrConnectTimeout=1 --set postgresql.repmgrReconnectInterval=2 --set pgpoolImage.debug=true pqs-tester bitnami/postgresql-ha

root@minikube01 ~ # k get pods
NAME                                               READY   STATUS    RESTARTS      AGE
pqs-tester-postgresql-ha-pgpool-7fb9bfdcf8-g7zjh   1/1     Running   0             3m32s
pqs-tester-postgresql-ha-postgresql-0              1/1     Running   0             3m32s
pqs-tester-postgresql-ha-postgresql-1              1/1     Running   0             3m31s
pqs-tester-postgresql-ha-postgresql-2              1/1     Running   0             3m31s


```
