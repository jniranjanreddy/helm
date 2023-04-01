```

helm install gloo-mgmt gloo-mesh-enterprise/gloo-mesh-enterprise \
--namespace gloo-mesh \
--kube-context ${MGMT_CONTEXT} \
--version ${GLOO_VERSION} \
--set global.cluster=$MGMT_CLUSTER \
--set mgmtClusterName=$MGMT_CLUSTER \
--set glooMeshLicenseKey=${GLOO_MESH_LICENSE_KEY} \
--set glooMeshMgmtServer.floatingUserId=true \
--set glooMeshUi.floatingUserId=true \
--set glooMeshRedis.floatingUserId=true

```
