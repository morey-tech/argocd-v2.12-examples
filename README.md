# What's New in Argo CD v2.12?

Try out the demos for yourself by opening up this repo in a Codespace, which will create a Kubernetes cluster with k3d and deploy Argo CD v2.12. You can then open the UI in a new window with port-forwarding from the Codespace, and use the pre-configured `argocd` CLI!

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/morey-tech/argocd-v2.12-examples)

Get the `admin` password for the UI from:

```bash
cat ~/argo-cd-admin-password.txt
```

## [1. Multi-Source Application Rollbacks](features/1-multi-source-rollbacks/README.md)

## [2. Sources Tab in Application Details](features/2-sources-tab/README.md)

## [3. Application Project Repository Credentials](features/3-app-project-repo-creds/README.md)

## [4. Application Labels on Kubernetes Events](features/4-app-labels-k8s-events/README.md)

## [5. Force Sync Application Annotation](features/5-force-sync-annotation/README.md)

## [6. Consistent-Hashing Algorithm](features/6-consistent-hashing-algorithm/README.md)
