# Install bootdev 
go install github.com/bootdotdev/bootdev@latest

# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

kubectl version --client

# Install minikube

curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

minikube start --extra-config="apiserver.cors-allowed-origins=['http://boot.dev']"

minikube dashboard --port=63840

minikube stop
minikube delete

# Deploy image to k8s

kubectl create deployment synergychat-web --image=docker.io/bootdotdev/synergychat-web:latest

kubectl get deployments

kubectl get pods

kubectl port-forward synergychat-web-f7b9f96dd-dljlv 8080:8080

kubectl edit deployment synergychat-web

kubectl logs synergychat-web-f7b9f96dd-dljlv 

# k8s pod network
kubectl get pods -o wide

http://127.0.0.1:8001/api/v1/namespaces/default/pods

# k8s deployment 
kubectl get deployment synergychat-web -o yaml

kubectl get deployment synergychat-web -o yaml > web-deployment.yaml

kubectl apply -f

# k8s configmaps
kubectl apply -f api-configmap.yaml

kubectl get configmaps

kubectl delete deployment synergychat-web

kubectl port-forward synergychat-api-865c8594b9-mtzfj 8080:8080

# k8s service

kubectl apply -f web-service.yaml

kubectl port-forward service/web-service 8080:80

kubectl get svc

# k8s gateway


kubectl apply --server-side -f https://github.com/envoyproxy/gateway/releases/download/v1.5.1/install.yaml

kubectl apply -f app-gatewayclass.yaml

kubectl apply -f  app-gateway.yaml

kubectl apply -f web-httproute.yaml

kubectl apply -f api-httproute.yaml

kubectl get pods -n envoy-gateway-system

vim /etc/hosts
192.168.49.2        synchat.internal
192.168.49.2        synchatapi.internal

ping synchat.internal

minikube tunnel --bind-address="127.0.0.1" -c