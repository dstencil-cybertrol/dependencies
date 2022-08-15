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
helm upgrade --install dependencies dependencies/dependencies --version "version to install" --namespace=kube-system --set metallb.addresses={"your metallb ip address or range"}
```

## Updating components

### metallb

When updating metallb, the operator manifest can be found from tagged versions [here.](https://github.com/metallb/metallb-operator/blob/v0.13.4/bin/metallb-operator.yaml)

Select the correct tag from the dropdown.

### FluxCD

Flux installation manifests can be updated by updating your flux cli and running `flux install --export`

Proxy information is added to the manager container in the source-controller deployment.

```
        {{- if .Values.flux.proxy.enabled }}
        - name: "HTTPS_PROXY"
          value: {{ .Values.flux.proxy.server | quote }}
        - name: "NO_PROXY"
          value: {{ .Values.flux.proxy.noProxy | quote }}
        {{- end }}
```

