

```
curl -sL https://run.solo.io/gloo/install | sh
export PATH=$HOME/.gloo/bin:$PATH
```

## Installing on Kubernetes with glooctl 

```
glooctl install gateway
```

## Installing on Kubernetes with Helm

```
helm repo add gloo https://storage.googleapis.com/solo-public-helm
helm repo update
kubectl create namespace my-namespace
```

