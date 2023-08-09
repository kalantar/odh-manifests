# Iter8

Iter8 is the Kubernetes release optimizer built for DevOps, MLOps, SRE and data science teams. Iter8 automates traffic control for new versions of apps/ML models in the cluster.

Iter8 comes with one component:

1. [Iter8](#Iter8)

## Iter8

Contains deployment manifests for the Iter8 controller.

- [iter8-controller](https://github.com/iter8-tools/iter8)

## Iter8 architecture

Iter8 has a controller that automatically reconfigures the Service Mesh when the set of versions of a model change. This capability enables blue-green and canary rollouts of new versions of a model.

## Installation process
Following are the steps to install Iter8 as a part of OpenDataHub install:

1. Install the OpenDataHub operator.
2. Create a KfDef that includes the Iter8 component.

```yaml
apiVersion: kfdef.apps.kubeflow.org/v1
kind: KfDef
metadata:
  name: opendatahub
  namespace: opendatahub
spec:
  applications:
    - kustomizeConfig:
        repoRef:
          name: manifests
          path: iter8/clusterScoped
      name: kserve
  repos:
    - name: manifests
      uri: https://api.github.com/repos/kalantar/odh-manifests/tarball/test
  version: master
```
3. Install Service Mesh component and configure them appropriately.
Provide details here.

## Using Iter8 in ODH

Iter8 can be used with Model Mesh Serving or with KServe.

### Model Mesh Serving

See [Iter8](https://iter8.tools) for [blue-green](https://iter8.tools/0.15/tutorials/integrations/kserve-mm/blue-green/) and [canary](https://iter8.tools/0.15/tutorials/integrations/kserve-mm/canary/) rollout.

### KServe

See [Iter8](https://iter8.tools) for [blue-green](https://iter8.tools/0.15/tutorials/integrations/kserve/blue-green/) and [canary](https://iter8.tools/0.15/tutorials/integrations/kserve/canary/) rollout.