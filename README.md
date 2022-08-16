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
helm upgrade --install dependencies dependencies/dependencies --namespace=kube-system --version "version to install"
```

## Updating components

First update the versions in helm/dependencies/Chart.yaml and then run: 

```bash
helm repo update
helm dependency update helm/dependencies
```

This will update the repos and then pull the charts matching the pinned versions.

### metallb

When updating metallb, the operator manifest can be found from tagged versions [here.](https://github.com/metallb/metallb-operator/blob/v0.13.4/bin/metallb-operator.yaml)

Select the correct tag from the dropdown.

### FluxCD

Flux installation manifests can be updated by updating your flux cli and running `flux install --export`

Proxy information is optionally added to the manager container in the source-controller deployment.

```
        {{- if .Values.flux.proxy.enabled }}
        - name: "HTTPS_PROXY"
          value: {{ .Values.flux.proxy.server | quote }}
        - name: "NO_PROXY"
          value: {{ .Values.flux.proxy.noProxy | quote }}
        {{- end }}
```

### cert-manager

Cert manager CRDs need to be updated in helm/dependencies/crds when updating the version of cert-manager.

cert-manager CRDs can be found [here.](https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.crds.yaml)

Select the correct version in the url for the version of cert-manager in this chart.
