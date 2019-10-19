```bash
kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml

kubectl get destinationrules -o yaml

kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml

kubectl get virtualservices

kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-test-v2.yaml

kubectl get virtualservice reviews -o yaml

kubectl delete -f samples/bookinfo/networking/virtual-service-all-v1.yaml
```

helm install install/kubernetes/helm/istio \
    --name istio \
    --tls \
    --namespace istio-system \
    --set global.mtls.enabled=true \
    --set grafana.enabled=true \
    --set servicegraph.enabled=true \
    --set tracing.enabled=true \
    --set kiali.enabled=true