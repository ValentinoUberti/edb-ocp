# Install edb using policy generator

```
kustomize build --enable-alpha-plugins edb-operator-install/overlays/ | oc apply -f -

ROLE=`kubectl get clusterrole | grep  cloud-native-postgresql | awk '{print $1}'`; echo $ROLE; kubectl patch clusterrole $ROLE --type='json' -p='[{"op": "add", "path": "/rules/-", "value": {"apiGroups": ["postgresql.k8s.enterprisedb.io"], "resources": ["clusterimagecatalogs"], "verbs": ["get", "list", "watch"]}}]'

kustomize build --enable-alpha-plugins edb-cluster-deploy/overlays/ | oc apply -f -
```

## Install cpn kubectl plugin
 - https://github.com/EnterpriseDB/kubectl-cnp

## Benchmark
 - https://github.com/EnterpriseDB/kubectl-cnp
 - done in 245.38 s (drop tables 0.43 s, create tables 0.01 s, client-side generate 183.28 s, vacuum 0.29 s, primary keys 61.36 s).


