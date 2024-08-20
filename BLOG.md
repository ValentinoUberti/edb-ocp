![EDB Logo](images/edb-logo.svg?raw=true "EDB")

Ditch the database drama and embrace the power of EDB - the cloud-native database that installs in minutes.

- Serverless, autoscaling database built for the cloud
- 99.99% availability with automated failover
- Seamless integration with OpenShift
- Easy to set up and manage
- Level 5 OLM Operator


EDB is perfect for developers and engineering teams who want to unlock the full potential of the cloud and of the on-premise OpenShift clusters as you will stop wasting cycles on complex setups and enjoy a hassle-free database experience that scales with your needs.

Also, EDB gives you a fully-managed, cloud-native database that just works. With the simplicity of ACM, you can have it up and running in no time on selected OpenShift clusters thanks to the ACM governance policies using the GitOps approach.

Thanks to the well know ACM policies is easy to apply any resources to a selected set of OpenShift clusters using labels. Multiple ACM policies applied in a specific order are required to install and configure an operator like the EDB one. The ACM policies dependencies feature is fully supported from ACM 2.7.

To simplify the policies creation process, the kustomize policy generator is being used as you can see from the example repo git.


The first step implies to install the operator it self executing the following command:

```
kustomize build --enable-alpha-plugins edb-operator-install/overlays/ | oc apply -f -
```

The ```edb-operator-install``` directory has the classic kustomize layout composed by a 'base' and an 'overlays' subdirectory


```
▒▓ Ͽ  …/edb-ocp/edb-operator-install   main   12:10  
❯ tree
.
├── base
│   ├── kustomization.yaml
│   ├── namespace.yaml
│   ├── operator-group.yaml
│   ├── policy-generator.yaml
│   └── sub.yaml
└── overlays
    ├── edb-1
    │   └── kustomization.yaml
    └── kustomization.yaml
```


Once the first command is run, the following resources will be created on all the OpenShift clusters having a the `vendor` label equal to 'OpenShift':

- Namespace
- Operator group
- Operator subscription

This is the policy-generator.yaml used in the example:

```
apiVersion: policy.open-cluster-management.io/v1
kind: PolicyGenerator
metadata:
  name: install-edb-operator
policyDefaults:
  namespace: policies
  placement:
    name: placement-edb
    clusterSelectors:
      vendor: "OpenShift"
  remediationAction: enforce
placementBindingDefaults:
  name: "binding-edb"
policies:
  - name: install-edb-operator
    manifests:
      - path: namespace.yaml
      - path: operator-group.yaml
      - path: sub.yaml
```

