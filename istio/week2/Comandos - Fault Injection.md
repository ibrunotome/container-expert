```bash
kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml
vim samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml apiVersion: networking.istio.io/v1alpha3 kind: VirtualService metadata: name: reviews spec: hosts: - reviews http: - match: - headers: end-user: exact: jason route: - destination: host: reviews subset: v2 - route: - destination: host: reviews subset: v1

kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml

vim samples/bookinfo/networking/virtual-service-ratings-test-delay.yaml apiVersion: networking.istio.io/v1alpha3 kind: VirtualService metadata: name: ratings spec: hosts: - ratings http: - match: - headers: end-user: exact: jason fault: delay: percentage: value: 100.0 fixedDelay: 7s route: - destination: host: ratings subset: v1 - route: - destination: host: ratings subset: v1

kubectl apply -f samples/bookinfo/networking/virtual-service-ratings-test-delay.yaml
kubectl get virtualservice ratings -o yaml

vim samples/bookinfo/networking/virtual-service-ratings-test-abort.yaml apiVersion: networking.istio.io/v1alpha3 kind: VirtualService metadata: name: ratings spec: hosts: - ratings http: - match: - headers: end-user: exact: jason fault: abort: percentage: value: 100.0 httpStatus: 500 route: - destination: host: ratings subset: v1 - route: - destination: host: ratings subset: v1

kubectl apply -f samples/bookinfo/networking/virtual-service-ratings-test-abort.yaml
kubectl delete -f samples/bookinfo/networking/virtual-service-all-v1.yaml
```
