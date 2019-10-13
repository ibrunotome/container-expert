```bash
curl -fsSL https://get.docker.com | bash

apt-get update && apt-get install -y apt-transport-https

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" >  /etc/apt/sources.list.d/kubernetes.list

apt-get update

apt-get install -y kubelet kubeadm kubectl

kubeadm config images pull

kubeadm init

mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

kubectl get nodes

curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.2.2 sh -

cd istio-1.2.2

export PATH=$PWD/bin:$PATH

for i in install/kubernetes/helm/istio-init/files/crd*yaml; do kubectl apply -f $i; done

kubectl api-resources | grep istio

kubectl get crd | grep istio

kubectl apply -f install/kubernetes/istio-demo.yaml

kubectl get svc -n istio-system

kubectl get pods -n istio-system

kubectl label namespace default istio-injection=enabled

kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml

```
