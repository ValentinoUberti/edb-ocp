# Install edb using policy generator

```
kustomize build --enable-alpha-plugins edb-operator-install/overlays/ | oc apply -f -

ROLE=`kubectl get clusterrole | grep  cloud-native-postgresql | awk '{print $1}'`; echo $ROLE; kubectl patch clusterrole $ROLE --type='json' -p='[{"op": "add", "path": "/rules/-", "value": {"apiGroups": ["postgresql.k8s.enterprisedb.io"], "resources": ["clusterimagecatalogs"], "verbs": ["get", "list", "watch"]}}]'

kustomize build --enable-alpha-plugins edb-cluster-deploy/overlays/ | oc apply -f -
```