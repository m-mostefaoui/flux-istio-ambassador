# ISTIO, AMBASSADOR, FLUX

## Install Tiller

```
kubectl create serviceaccount -n kube-system tiller
```

```
kubectl create clusterrolebinding tiller-binding --clusterrole=cluster-admin --serviceaccount kube-system:tiller
```

```
helm init --service-account tiller
```

## Install Weave Flux with Helm

```
helm repo add weaveworks https://weaveworks.github.io/flux
```

```
helm install --name flux \
--set helmOperator.create=true \
--set git.url=git@github.com:m-mostefaoui/flux-istio-ambassador \
--set git.chartsPath=charts \
--namespace flux \
weaveworks/flux
```

## Setup Git sync
At startup, Flux generates a SSH key and logs the public key. Find the SSH public key with:

```
kubectl -n flux logs deployment/flux | grep identity.pub
```

## Install Istio with Weave Flux


## Download & Setup Istio

### Download Istio

Let’s download the latest Istio
```
curl -L https://git.io/getLatestIstio | sh -
```

### Install Istio
```
kubectl apply -f  istio-1.0.2\install\kubernetes\istio-demo.yaml
```

Let’s make sure Istio related pods and services are running:

```
kubectl get pod,svc -n istio-system
```

Let's use `istioctl` command and inject the sidecar proxy when we you create the application pod

```
kubectl create -f <(istioctl kube-inject -f workloads/ga-deployment.yaml
```
