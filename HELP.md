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

