# Hello World Module
## A Helm Chart for an example Mesh for Data module

## Introduction

This helm chart defines a common structure to deploy a Kubernetes job for an M4D module.

The configuration for the chart is in the values file.

## Prerequisites

- Kubernetes cluster 1.10+
- Helm 3.0.0+

## Installation

### Modify values in Makefile

In `Makefile`:
- Change `DOCKER_USERNAME`, `DOCKER_PASSWORD`, `DOCKER_HOSTNAME`, `DOCKER_NAMESPACE`, `DOCKER_TAGNAME`, `DOCKER_IMG_NAME`, `DOCKER_CHART_IMG_NAME` to your own preferences.

### Build Docker image for Python application
```bash
make docker-build
```

### Push Docker image to your preferred container registry
```bash
make docker-push
```

### Configure the chart

- Settings can configured by editing the `values.yaml` directly.
- Change repository in `values.yaml` to your preferred Docker image. 
- Modify copy/read action as needed with appropriate values. 

### Login to Helm registry
```bash
make helm-login
```

### Lint and install Helm chart
```bash
make helm-verify
```

### Push the Helm chart

```bash
make helm-chart-push
```

## Uninstallation
```bash
make helm-uninstall
```

## Deploy M4D application which triggers module (WIP)
- In `hello-world-module.yaml`:
  * Change `spec.chart.name` to your preferred chart image.
  * Define `flows` and `capabilities` for your module. 

Deploy `M4DModule` in `m4d-system` namespace:
```bash
oc project m4d-system
oc create -f hello-world-module.yaml
```
- In `m4dapplication.yaml`:
  - Change `metadata.name` to your application name.
  - Define `appInfo.purpose`, `appInfo.role`, and `spec.data`
  - This ensures that a copy is triggered:
    ```yaml
    copy:
      required:true
    ```
- Deploy `M4DApplication` in `default` namespace:
```bash
oc project default
oc create -f m4dapplication.yaml
```
- Check if `M4DApplication` successfully deployed:
```bash
oc describe M4DApplication hello-world-module-test
```

- Check if module was triggered in `m4d-blueprints`:
```bash
oc project m4d-blueprints
oc get job
oc get pods
```



