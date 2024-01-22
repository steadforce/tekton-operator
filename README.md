## Dependencies

This chart expects the `tekton-operator` as a dependency. The used version
is the commit [5e09408bdf96b6e6bdd1535561540fc002c92c61](https://github.com/tektoncd/operator/commit/5e09408bdf96b6e6bdd1535561540fc002c92c61). The version specified in `Chart.yaml` is `0.69.99` does actually not exist as the current release `0.69.1` has a bug which prevents the operator webhook from starting. Thus the latest commit from the main branch was taken.

See the [Helm docs](https://helm.sh/docs/topics/charts/#chart-dependencies)
for details.

## Render helm charts locally

The following command renders the charts like argo-cd does to validate the content.

```
 helm template --release-name tekton-operator -n tekton-operator --include-crds --skip-tests \
  -a operator.tekton.dev/v1alpha1 \
  -a security.istio.io/v1beta1 \
  --output-dir _renderOutput . 
```

You can use this command to check if the output is as you expect. The `-a` parameters are needed since we use the
helm feature `.Capabilities.APIVersions.Has` to determine if a `CR` is installable in the cluster or not. Since
helm templating is designed to work offline we have to list the supported `CR`. Using `.Capabilities.APIVersions.Has`
feature in templating prevents sync errors in argo-cd if a `CR` can't be applied since its `CRD` isn't ready.