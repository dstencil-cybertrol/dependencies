# dependencies
 Helm chart to install and manage dependencies.

This helm chart contains the following components:

- cert-manager
- cilium
- coredns
- ingress-nginx
- longhorn
- sealed-secrets
- metallb (operator)
- system-upgrade-controller


This helm chart requires being installed in two stages, as dependencies for some components require other components to be deployed.

## Installation

```bash
helm repo add dependencies https://cybertrol-engineering.github.io/dependencies/
helm install dependencies dependencies/dependencies --version "version to install" --namespace=kube-system --set metallb.addresses={"your metallb ip address or range"}
```
