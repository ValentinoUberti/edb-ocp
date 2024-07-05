# Install edb using policy generator

```
kustomize build --enable-alpha-plugins edb-operator-install/overlays/ | oc apply -f -
kustomize build --enable-alpha-plugins edb-cluster-deploy/overlays/ | oc apply -f -
```