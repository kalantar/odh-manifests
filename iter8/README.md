# Iter8

Iter8 comes with one component:

1. [Iter8](#Iter8)

## Iter8

Contains deployment manifests for the Iter8 controller.

- [iter8-controller](https://github.com/iter8-tools/iter8)
  - forked upstream iter8-tools/iter8 repository

## Iter8 architecture

## Iter8 manifests

## Installation process
Following are the steps to install Model Mesh as a part of OpenDataHub install:

1. Install the OpenDataHub operator.
2. Make sure you install Service Mesh and Serverless components and configure them appropriately.
See [OCP official instructions](https://docs.openshift.com/serverless/1.29/integrations/serverless-ossm-setup.html) and the 
3. related documentation for [KServe on Openshift](https://github.com/kserve/kserve/blob/master/docs/OPENSHIFT_GUIDE.md#installation-with-service-mesh) from the kserve repo for more.
4. Create a KfDef that includes the Iter8 component.

```
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
          path: iter8/namespaceScoped
      name: kserve
  repos:
    - name: manifests
      uri: https://api.github.com/repos/opendatahub-io/odh-manifests/tarball/master
  version: master
```

5. You can now create a new project.
6. Make sure that you have a runtime defined in your target namespace (you can use a [template](https://github.com/opendatahub-io/odh-dashboard/blob/main/manifests/modelserving/ovms-ootb.yaml) in ODH).

```yaml
apiVersion: serving.kserve.io/v1alpha1
kind: ServingRuntime
metadata:
  name: example-runtime
spec:
...
```
More information in the [KServe docs](https://kserve.github.io/website/0.10/modelserving/servingruntimes/).

7. Create an `InferenceService` CR in your target namespace.


## Using KServe in ODH

You can use the `InferenceService` examples from KServe. Make sure to include the additional annotation for OpenShift Service Mesh:

```yaml
metadata:
  annotations:
    sidecar.istio.io/inject: "true"
    sidecar.istio.io/rewriteAppHTTPProbers: "true"
    serving.knative.openshift.io/enablePassthrough: "true"
```

Example:

```yaml
apiVersion: "serving.kserve.io/v1beta1"
kind: "InferenceService"
metadata:
  name: "sklearn-iris"
  namespace: kserve-demo
  annotations:
    sidecar.istio.io/inject: "true"
    sidecar.istio.io/rewriteAppHTTPProbers: "true"
    serving.knative.openshift.io/enablePassthrough: "true"
spec:
  predictor:
    model:
      runtime: <your-runtime>
      modelFormat:
        name: sklearn
      storageUri: "gs://kfserving-examples/models/sklearn/1.0/model"
```


