## How to install Postgresql HA

```


helm search repo bitnami/postgresql

NAME                    CHART VERSION   APP VERSION     DESCRIPTION
bitnami/postgresql      12.2.8          15.2.0          PostgreSQL (Postgres) is an open source object-...
bitnami/postgresql-ha   11.3.0          15.2.0          This PostgreSQL cluster solution includes the P...



root@minikube01 ~ # helm install postgresql bitnami/postgresql-ha
NAME: postgresql
LAST DEPLOYED: Thu Apr 20 10:31:30 2023
NAMESPACE: cafe
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: postgresql-ha
CHART VERSION: 11.3.0
APP VERSION: 15.2.0
** Please be patient while the chart is being deployed **
PostgreSQL can be accessed through Pgpool via port 5432 on the following DNS name from within your cluster:

    postgresql-postgresql-ha-pgpool.cafe.svc.cluster.local

Pgpool acts as a load balancer for PostgreSQL and forward read/write connections to the primary node while read-only connections are forwarded to standby nodes.

To get the password for "postgres" run:

    export POSTGRES_PASSWORD=$(kubectl get secret --namespace cafe postgresql-postgresql-ha-postgresql -o jsonpath="{.data.password}" | base64 -d)

To get the password for "repmgr" run:

    export REPMGR_PASSWORD=$(kubectl get secret --namespace cafe postgresql-postgresql-ha-postgresql -o jsonpath="{.data.repmgr-password}" | base64 -d)

To connect to your database run the following command:

    kubectl run postgresql-postgresql-ha-client --rm --tty -i --restart='Never' --namespace cafe --image docker.io/bitnami/postgresql-repmgr:15.2.0-debian-11-r16 --env="PGPASSWORD=$POSTGRES_PASSWORD"  \
        --command -- psql -h postgresql-postgresql-ha-pgpool -p 5432 -U postgres -d postgres

To connect to your database from outside the cluster execute the following commands:

    kubectl port-forward --namespace cafe svc/postgresql-postgresql-ha-pgpool 5432:5432 &
    psql -h 127.0.0.1 -p 5432 -U postgres -d postgres
```
