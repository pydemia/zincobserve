# ZincObserve

See: https://github.com/zinclabs/zincobserve

Helm: https://github.com/zinclabs/zincobserve-helm-chart

## Pre-requisite

* `kind-config.yaml`

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  extraMounts:
   - hostPath: /home/xxx/.ssl/cacert.crt
     containerPath: /usr/local/share/ca-certificates/cacert.crt
    #  containerPath: /usr/share/ca-certificates/cacert.crt
```

```bash
kind create cluster \
  --name kind-local \
  --config kind-config.yaml

docker exec -it kind-local-control-plane /bin/bash -c "update-ca-certificates"
```

## Install

```bash
helm repo add zinc https://charts.zinc.dev
helm repo update

helm search repo zinc/zincobserve
helm pull zinc/zincobserve --untar
#   --version 0.4.1 \

```

```bash
kubectl create ns zincobserve

helm install zincobserve zinc/zincobserve \
  --namespace zincobserve \
  -f custom-values.yaml
```

## Connect

```bash
kubectl -n zincobserve port-forward svc/zincobserve-router 5080:5080
```
