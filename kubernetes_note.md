# Kubernetes note

## Installation

```bash
brew install kubectl
```

## Usage

```bash
kubectl config get-contexts
kubectl config use-context CONTEXT_NAME

kubectl get all
kubectl get all -n $NAMESPACE

# List all namespaces
kubectl get namespace

# List pods
kubectl get pods --all-namespaces
kubectl get pods -n $NAMESPACE

# Services
kubectl get svc

# Ingress
kubectl get ingress
kubectl get ingress -n $NAMESPACE
kubectl describe ingress $INGRESS_NAME -n $NAMESPACE

# Logs
kubectl logs -f -n $NAMESPACE services/$SERVICE_NAME
kubectl logs -f -l app.kubernetes.io/name=$LABEL_NAME -n $NAMESPACE

# execute command remote
kubectl exec -n postgres postgres-0 -- df -h

# Force to delete Running pods with namespase
kubectl get pods -n $NAMESPACE --field-selector 'status.phase!=Running' -o json | kubectl delete -f -

# Delete job
kubectl delete job $JOB_NAME -n $NAMESPACE

# Porting forward
kubectl port-forward --namespace $NAMESPACE services/$SERVICE_NAME 8080:8080

# Porting forward by kubefwd
# https://github.com/txn2/kubefwd
# > brew install txn2/tap/kubefwd
sudo kubefwd services -n $NAMESPACE

# Check cronjobs and jobs
kubectl get cronjobs -n $NAMESPACE
kubectl get jobs -n $NAMESPACE

# Check DNS pod is running
kubectl get pods --namespace=kube-system -l k8s-app=kube-dns

# rollout status
kubectl rollout status deployment/$DEPLOYMENT_NAME -n $NAMESPACE

# rollout history
kubectl rollout history deployment/$DEPLOYMENT_NAME -n $NAMESPACE

# rollback
kubectl rollout undo deployment/$DEPLOYMENT_NAME -n $NAMESPACE

# secret
kubectl get secret
kubectl get secret -n $NAMESPACE

# events
kubectl get events

# Show deployments
kubectl get deploy -n $NAMESPACE

# Deploy image
kubectl set image deployment $DEPLOYMENT_NAME $CONTAINER_NAME=$IMAGE_PATH:latest -n $NAMESPACE

# Wait unit pod ready (example)
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=120s

# Check apiVersion
kubectl api-resources
```

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
