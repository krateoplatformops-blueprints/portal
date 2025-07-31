# *Portal* Blueprint

## Purpose

The **Portal** Blueprint provides a foundational setup to deploy and customize the [Krateo Composable Portal](https://github.com/krateoplatformops/frontend) using Kubernetes-native resources. This Blueprint includes a Helm chart to help you bootstrap your own Krateo Composable Portal experience.

## What It Does

- Deploys an initial consistent experience of Krateo Composable Portal

## Usage

### Install the Helm Chart

Download Helm Chart values:

```sh
helm repo add marketplace https://marketplace.krateo.io
helm repo update marketplace
helm inspect values marketplace/portal --version 0.0.1 > ~/portal-values.yaml
```

Modify the *portal-values.yaml* file as the following example:

```yaml
enableAdminUser: true
enableCyberjokerUser: true
enableDemoSystemNamespace: true
```

Install the Blueprint:

```sh
helm install <name> template \
  --repo https://marketplace.krateo.io \
  --namespace <namespace> \
  --create-namespace \
  -f ~/portal-values.yaml
  --version 0.0.1 \
  --wait
```

### Install using Krateo Composable Operation

Install the CompositionDefinition for the *Blueprint*:

```sh
cat <<EOF | kubectl apply -f -
apiVersion: core.krateo.io/v1alpha1
kind: CompositionDefinition
metadata:
  name: portal
  namespace: krateo-system
spec:
  chart:
    repo: portal
    url: https://marketplace.krateo.io
    version: 0.0.1
EOF
```

Install the Blueprint using, as metadata.name, the *Blueprint* name (the Helm Chart name of the blueprint):

```sh
cat <<EOF | kubectl apply -f -
apiVersion: composition.krateo.io/v0-0-1
kind: Portal
metadata:
  name: <name>
  namespace: <namespace>
spec:
    enableAdminUser: true
    enableCyberjokerUser: true
    enableDemoSystemNamespace: true
EOF
```