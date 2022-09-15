# Kubernetes note

## Minikube

minikube implements a local Kubernetes cluster on macOS, Linux, and Windows.

<https://github.com/kubernetes/minikube>

```bash
brew install kubernetes-cli
brew cask install minikube
minikube start
kubectl cluster-info
```

### Start hello-minikube app

```bash
kubectl run hello-minikube \
  --generator=run-pod/v1 \
  --image=k8s.gcr.io/echoserver:1.10 \
  --port=8080
```

```bash
kubectl expose deployment hello-minikube --type=NodePort
minikube service hello-minikube --url
```

### Start pod

```bash
kubectl create -f pod.yaml
kubectl get pods
kubectl describe pod myapp-pod
kubectl delete pod myapp-pod

# method 1. forward port 
kubectl port-forward myapp-pod 8088:8000
open http://localhost:8088

# method 2. create service
kubectl expose pod myapp-pod --type=NodePort --name=myapp-pod-service
minikube service myapp-pod-service --url

# Attach to process
kubectl attach myapp-pod -i

# execute command in pod
kubectl exec myapp-pod -- ls

# Show labels
kubectl get pods --show-labels

# Add label
kubectl label pods myapp-pod version=latest

# Run a alpine for debug same cluster
kubectl run -i --tty alpine --image=alpine --restart=Never -- sh
```
