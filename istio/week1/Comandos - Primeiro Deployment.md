```bash
kubectl label namespace default istio-injection=enabled

kubectl apply -f bookinfo.yaml

kubectl apply -f bookinfo-gateway.yaml

istioctl get gateway

kubectl get gateway

kubectl get virtualservices -o yaml

kubectl get services

kubectl get pods

kubectl get virtualservices.networking.istio.io bookinfo -o yaml

kubectl get virtualservices.networking.istio.io

kubectl describe virtualservices.networking.istio.io bookinfo

kubectl get gateways.networking.istio.io

kubectl describe gateways.networking.istio.io bookinfo-gateway

kubectl port-forward svc/productpage 9080:9080 -n default --address=0.0.0.0
```
