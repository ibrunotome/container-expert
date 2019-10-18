```bash
kubectl get pods -n istio-system

kubectl get services -n istio-system

kubectl edit service kiali -n istio-system

kubectl port-forward svc/kiali 20001:20001 -n istio-system --address=0.0.0.0

kubectl port-forward svc/grafana 3000:3000 -n istio-system --address=0.0.0.0
```
