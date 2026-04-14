# Flux Review Layout

This directory is a review-only Flux GitOps scaffold for the current k3s cluster.

Nothing in this directory has been applied to the cluster.

## Layout

- `clusters/production/flux-system/`: Flux install and cluster sync objects
- `infrastructure/`: shared platform resources
- `apps/production/`: application workloads reconciled by Flux

## Assumptions

- Cluster name: `production`
- Git branch: `main`
- Git repo URL: `ssh://git@github.com/Turtlez32/flux-repo.git`
- Flux namespace: `flux-system`
- Source path synced into the cluster: `./clusters/production`

## Review Notes

Before applying this, you should review:

- `clusters/production/flux-system/gotk-sync.yaml`
- `clusters/production/kustomization.yaml`
- `apps/production/headlamp/`
- `infrastructure/controllers/`

## What Is Missing On Purpose

- Full version-pinned Flux controller install export in `gotk-components.yaml`
- Git repository URL and auth secret
- SOPS/age decryption
- image automation
- notification providers
- multi-tenancy lockdown

## Suggested Next Step

After review, replace the placeholder Git URL in `gotk-sync.yaml`, then either:

- export the controller install manifests with a pinned Flux CLI version into `gotk-components.yaml`
- then apply `kubectl apply -k flux/clusters/production/flux-system`
- or bootstrap with the Flux CLI and keep this repo structure as the source of truth

## Secret Handling

- `clusters/production/flux-system/secret.sops.yaml` is the Git-safe encrypted version of the Flux Git auth secret
- `clusters/production/flux-system/secret-review.yaml` is plaintext and should not be committed
- `age.agekey` is the recovery key and should be backed up outside Git

For first-time bootstrap, the Flux Git auth secret usually needs to be created by hand or via CLI before Flux can pull and decrypt from Git.
