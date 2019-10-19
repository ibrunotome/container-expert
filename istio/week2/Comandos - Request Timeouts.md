```bash
kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml
# vim virtual-service-reviews-normal.yaml apiVersion: networking.istio.io/v1alpha3 kind: VirtualService metadata: name: reviews spec: hosts: - reviews http: - route: - destination: host: reviews subset: v2

kubectl apply -f virtual-service-normal.yaml

# vim virtual-service-ratings-delay.yaml apiVersion: networking.istio.io/v1alpha3 kind: VirtualService metadata: name: ratings spec: hosts: - ratings http: - fault: delay: percent: 100 fixedDelay: 2s route: - destination: host: ratings subset: v1

kubectl apply -f virtual-service-ratings-delay.yaml

# vim virtual-service-reviews-timeout.yaml apiVersion: networking.istio.io/v1alpha3 kind: VirtualService metadata: name: reviews spec: hosts: - reviews http: - route: - destination: host: reviews subset: v2 timeout: 0.5s

kubectl apply -f virtual-service-reviews-timeout.yaml
kubectl delete -f samples/bookinfo/networking/virtual-service-all-v1.yaml
```
