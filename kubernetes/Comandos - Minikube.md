Portas que devemos nos preocupar: MASTER kube-apiserver => 6443 TCP etcd server API => 2379-2380 TCP Kubelet API => 10250 TCP kube-scheduler => 10251 TCP kube-controller-manager => 10252 TCP Kubelet API Read-only => 10255 TCP WORKER Kubelet API => 10250 TCP Kubelet API Read-only => 10255 TCP NodePort Services => 30000-32767 TCP

```bash
Antes de instalar o minikube, precisamos realizar a instalação do kubectl:

LINUX

# curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

# chmod +x kubectl && mv kubectl /usr/local/bin/


MACOS
# curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/darwin/amd64/kubectl

# chmod +x kubectl && mv kubectl /usr/local/bin/

No MacOS, você pode também realizar a instalação através do brew:
# brew install kubectl

WINDOWS

# curl -Lo https://storage.googleapis.com/kubernetes-release/release/v1.13.7/bin/windows/amd64/kubectl.exe

Se você utiliza o PSGallery:

# Install-Script -Name install-kubectl -Scope CurrentUser -Force
install-kubectl.ps1 [-DownloadLocation <path>]

doc:
https://kubernetes.io/docs/tasks/tools/install-minikube/

LINUX
# curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \ && chmod +x minikube
# sudo cp minikube /usr/local/bin && rm minikube


MACOS
# curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64 \ && chmod +x minikube
# sudo cp minikube /usr/local/bin && rm minikube


WINDOWS
https://storage.googleapis.com/minikube/releases/v1.1.1/minikube-windows-amd64.exe
```

```bash
# minikube start
# kubectl get nodes
# minikube ip
# minikube ssh
# minikube start
# minikube stop
# minikube dashboard
# minikube logs
```
