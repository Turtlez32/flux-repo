# TrueNAS CSI

This directory scaffolds a Flux-managed TrueNAS iSCSI storage integration for the
production k3s cluster.

## What is included

- `namespace.yaml`: dedicated `storage` namespace
- `repository.yaml`: `democratic-csi` Helm chart repository
- `truenas-iscsi-helmrelease.yaml`: suspended Helm release for the TrueNAS iSCSI driver
- `secret.sops.example.yaml`: example values secret to copy into a real encrypted `secret.sops.yaml`

## Before enabling

1. Install iSCSI tooling on every k3s node:
   - Ubuntu: `sudo apt-get install -y open-iscsi`
   - Enable it: `sudo systemctl enable --now iscsid`
2. Create the backing path on TrueNAS:
   - `tank/k8s/iscsi/v` for provisioned zvols
   - `tank/k8s/iscsi/s` for detached snapshots
3. Confirm the TrueNAS iSCSI portal group and initiator group IDs.
4. Create a TrueNAS API key with permissions to manage datasets and iSCSI assets.
5. Copy `secret.sops.example.yaml` to `secret.sops.yaml`, replace placeholders, encrypt it with `sops`, and add it to this directory's `kustomization.yaml`.
6. Remove `spec.suspend: true` from `truenas-iscsi-helmrelease.yaml` once the secret is ready.

## Notes

- This setup intentionally does not replace the cluster default storage class.
- `freenas-api-iscsi` is chosen to avoid SSH-based provisioning and to keep the
  TrueNAS integration centered on the API endpoint at `fs.turtleware.au`.
- The example storage class is aimed at per-app databases and other `ReadWriteOnce`
  workloads.
