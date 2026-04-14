# Flux Bootstrap Example

This repo scaffold is intended to work with a standard Flux bootstrap flow after you replace the placeholder Git URL.

Example:

```bash
flux bootstrap github \
  --owner=Turtlez32 \
  --repository=flux-repo \
  --branch=main \
  --path=clusters/production \
  --personal
```

If you do not want Flux to create the repo contents for you, you can also:

```bash
kubectl apply -k flux/clusters/production/flux-system
```

Then create the `flux-system` auth secret separately for your Git provider.
