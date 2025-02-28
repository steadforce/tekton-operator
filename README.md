## Dependencies

This chart expects the `tekton-operator` as a dependency. The used has to be
specified in `Chart.yaml` in the `dependencies` section and by git reference tag
from the chart's source repository.

To get the helm chart fetched from sources, the helm git plugin is needed,
since there is still no helm release for this chart.
If you changed the versions (tag and chart version) for an update,
you need to run

    $ helm dependency update

in order to have the chart downloaded to the `charts` directory
and then also commit that new version alongside with the altered
`Chart.yaml` file.

You need the helm git plugin installed to be able to execute above command without
errors

See the [Helm docs](https://helm.sh/docs/topics/charts/#chart-dependencies)
and [Helm git](https://github.com/aslafy-z/helm-git) for details.

## Render helm charts locally

The following command renders the charts like argo-cd does to validate the content.

```
 helm template \
  --include-crds \
  --output-dir _local/local \
  --release-name tekton-operator \
  --skip-tests \
  -a operator.tekton.dev/v1alpha1 \
  -a security.istio.io/v1beta1 \
  -n tekton-operator \
  . 
```

You can use this command to check if the output is as you expect. The `-a` parameters are needed since we use the
helm feature `.Capabilities.APIVersions.Has` to determine if a `CR` is installable in the cluster or not. Since
helm templating is designed to work offline we have to list the supported `CR`. Using `.Capabilities.APIVersions.Has`
feature in templating prevents sync errors in argo-cd if a `CR` can't be applied since its `CRD` isn't ready.