## Basic Commands

```
helm repo list
helm repo add
helm repo update
helm search repo
helm search repo wildfly --versions
helm install
helm list
helm uninstall
```
## Helm Upgrade with set option
```
helm upgrade
helm history
helm status
helm list --superseded
helm list --deployed
helm list myapp1 -o yaml
Error: "helm list" accepts no arguments

Usage:  helm list [flags]
root@GZN00000333:/mnt/c/Users/NJannapureddy/helm# helm list -o yaml
- app_version: 1.0.0
  chart: mychart1-0.1.0
  name: myapp1
  namespace: default
  revision: "1"
  status: deployed
  updated: 2024-01-11 11:59:10.640726528 +0530 IST

# helm upgrade myapp1 stacksimplify/mychart1 --set "image.tag=2.0.0"
Release "myapp1" has been upgraded. Happy Helming!
NAME: myapp1
LAST DEPLOYED: Thu Jan 11 18:25:39 2024
NAMESPACE: default
STATUS: deployed
REVISION: 2
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services myapp1-mychart1)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
```
##  Install Helm Chart by specifying Chart Version
```
helm install myapp101 stacksimplify/mychart2 --version "CHART-VERSION"
helm install myapp101 stacksimplify/mychart2 --version "0.1.0"

# helm status myapp101 --show-resources
NAME: myapp101
LAST DEPLOYED: Thu Jan 11 20:48:41 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
RESOURCES:
==> v1/Service
NAME                TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
myapp101-mychart2   NodePort   10.98.171.138   <none>        80:31232/TCP   14s

==> v1/Deployment
NAME                READY   UP-TO-DATE   AVAILABLE   AGE
myapp101-mychart2   1/1     1            1           14s

==> v1/Pod(related)
NAME                                 READY   STATUS    RESTARTS   AGE
myapp101-mychart2-7654b8c4c4-gnvzx   1/1     Running   0          14s

root@GZN00000333:~# helm ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART          APP VERSION
myapp1          default         4               2024-01-11 18:41:03.433731022 +0530 IST deployed        mychart1-0.1.0 1.0.0
myapp101        default         1               2024-01-11 20:48:41.114286803 +0530 IST deployed        mychart2-0.1.0 1.0.0
myapp2          default         1               2024-01-11 11:59:23.301971641 +0530 IST failed          mychart1-0.1.0 1.0.0
mynginx         default         1               2024-01-11 11:42:13.87798132 +0530 IST  deployed        nginx-15.6.0   1.25.3



root@GZN00000333:~# helm status myapp1 --show-resources
NAME: myapp1
LAST DEPLOYED: Thu Jan 11 18:41:03 2024
NAMESPACE: default
STATUS: deployed
REVISION: 4
RESOURCES:
==> v1/Service
NAME              TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
myapp1-mychart1   NodePort   10.101.85.178   <none>        80:31231/TCP   35h

==> v1/Deployment
NAME              READY   UP-TO-DATE   AVAILABLE   AGE
myapp1-mychart1   1/1     1            1           35h

==> v1/Pod(related)
NAME                               READY   STATUS    RESTARTS      AGE
myapp1-mychart1-6b5488b97d-rmm5x   1/1     Running   1 (24h ago)   29h


NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services myapp1-mychart1)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT

root@GZN00000333:~# helm status myapp1 --revision 1
NAME: myapp1
LAST DEPLOYED: Thu Jan 11 11:59:10 2024
NAMESPACE: default
STATUS: superseded
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services myapp1-mychart1)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
root@GZN00000333:~# helm status myapp1 --revision 2
NAME: myapp1
LAST DEPLOYED: Thu Jan 11 18:25:39 2024
NAMESPACE: default
STATUS: superseded
REVISION: 2
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services myapp1-mychart1)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
root@GZN00000333:~# helm status myapp1 --revision 3
NAME: myapp1
LAST DEPLOYED: Thu Jan 11 18:39:30 2024
NAMESPACE: default
STATUS: superseded
REVISION: 3
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services myapp1-mychart1)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT

```
## uninstall
```
root@GZN00000333:~# helm uninstall mynginx
release "mynginx" uninstalled
root@GZN00000333:~# helm ls
NAME    NAMESPACE       REVISION        UPDATED STATUS  CHART   APP VERSION


root@GZN00000333:~# helm uninstall mynginx
release "mynginx" uninstalled

root@GZN00000333:~# helm ls
NAME    NAMESPACE       REVISION        UPDATED STATUS  CHART   APP VERSION

root@GZN00000333:~# helm history myapp101
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION
1               Sat Jan 13 00:15:04 2024        superseded      mychart2-0.1.0  1.0.0           Install complete
2               Sat Jan 13 00:20:57 2024        superseded      mychart2-0.2.0  2.0.0           Upgrade complete
3               Sat Jan 13 00:23:13 2024        superseded      mychart2-0.4.0  4.0.0           Upgrade complete
4               Sat Jan 13 00:27:37 2024        `uninstalled`   mychart2-0.2.0  2.0.0           Uninstallation complete


root@GZN00000333:~# helm rollback myapp101 3
Rollback was a success! Happy Helming!
root@GZN00000333:~# helm ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART          APP VERSION
myapp101        default         5               2024-01-13 00:38:17.480249686 +0530 IST deployed        mychart2-0.4.0 4.0.0
root@GZN00000333:~# helm history myapp101
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION
1               Sat Jan 13 00:15:04 2024        superseded      mychart2-0.1.0  1.0.0           Install complete
2               Sat Jan 13 00:20:57 2024        superseded      mychart2-0.2.0  2.0.0           Upgrade complete
3               Sat Jan 13 00:23:13 2024        superseded      mychart2-0.4.0  4.0.0           Upgrade complete
4               Sat Jan 13 00:27:37 2024        uninstalled     mychart2-0.2.0  2.0.0           Uninstallation complete
5               Sat Jan 13 00:38:17 2024        deployed        mychart2-0.4.0  4.0.0           Rollback to 3
```
## Dry run
```
helm install myapp901 stacksimplify/mychart1 --set service.nodePort=31240 --dry-run
helm install myapp901 stacksimplify/mychart1 --set service.nodePort=31240 --dry-run --debug
helm upgrade myapp901 stacksimplify/mychart1 -f myvalues.yaml --dry-run --debug
helm status myapp901 --show-resources
helm get values myapp901
helm get values myapp901
```
## Heml Manifest
```
# helm get manifest
helm get manifest RELEASE-NAME
helm get manifest myapp901

# helm get manifest --revision
helm get manifest RELEASE-NAME --revision int
helm get manifest myapp901 --revision 1


```



