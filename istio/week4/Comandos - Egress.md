```bash
cd istio-1.2.2/
kubectl apply -f samples/sleep/sleep.yaml
kubectl get deployments.
kubectl get pods
kubectl get configmaps istio -n istio-system
kubectl get configmaps istio -n istio-system -o yaml | grep -o "mode: ALLOW_ANY"
kubectl exec -ti sleep-7d457d69b5-wbcbw -c sleep -- curl -I https://www.linuxtips.io
kubectl edit configmaps istio -n istio-system
kubectl get configmaps istio -n istio-system -o yaml | sed 's/mode: ALLOW_ANY/mode: REGISTRY_ONLY/g' | kubectl replace -n istio-system -f -
kubectl get configmaps istio -n istio-system -o yaml | sed 's/mode: REGISTRY_ONLY/mode: ALLOW_ANY/g' | kubectl replace -n istio-system -f -
kubectl apply -f libera_httpbin_https.yaml
kubectl apply -f libera_httpbin.yaml
kubectl apply -f vs_httpbin_org.yaml
kubectl exec -ti sleep-7d457d69b5-wbcbw -c sleep -- curl -I http://httpbin.org/delay/2
kubectl exec -ti sleep-7d457d69b5-wbcbw -c sleep -- curl -I http://httpbin.org/delay/4
kubectl exec -ti sleep-7d457d69b5-wbcbw -c sleep -- curl -I http://httpbin.org/delay/3

# vim libera_httpbin.yaml

apiVersion: networking.istio.io/v1alpha3
kind: ServiceEntry
metadata:
  name: httpbin-ext
spec:
  hosts:
  - httpbin.org
  ports:
  - number: 80
    name: http
    protocol: HTTP
  resolution: DNS
  location: MESH_EXTERNAL


#vim libera_httpbin_https.yaml

apiVersion: networking.istio.io/v1alpha3
kind: ServiceEntry
metadata:
  name: google
spec:
  hosts:
  - www.google.com
  ports:
  - number: 443
    name: https
    protocol: HTTPS
  resolution: DNS
  location: MESH_EXTERNAL

# vim vs_httpbin_org.yaml

apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: httpbin-ext
spec:
  hosts:
    - httpbin.org
  http:
  - timeout: 3s
    route:
      - destination:
          host: httpbin.org
        weight: 100

```
