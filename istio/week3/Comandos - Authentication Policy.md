```bash
kubectl create ns giropops

kubectl create ns strigus

kubectl create ns girus

kubectl label namespace strigus istio-injection=enabled

kubectl label namespace giropops istio-injection=enabled

cd istio-1.2.2/

kubectl apply -f samples/httpbin/httpbin.yaml -n giropops

kubectl apply -f samples/httpbin/httpbin.yaml -n strigus

kubectl apply -f samples/httpbin/httpbin.yaml -n girus

kubectl apply -f samples/sleep/sleep.yaml -n giropops

kubectl apply -f samples/sleep/sleep.yaml -n strigus

kubectl apply -f samples/sleep/sleep.yaml -n girus

kubectl get pods --all-namespaces

kubectl exec -ti -n strigus sleep-7d457d69b5-h8frp -- curl http://httpbin.giropops:8000/ip -s -o /dev/null -w "%{http_code}\n"

kubectl exec -ti -n giropopssleep-7d457d69b5-tw5pt -- curl http://httpbin.strigus:8000/ip -s -o /dev/null -w "%{http_code}\n"

kubectl exec -ti -n giropops sleep-7d457d69b5-tw5pt -- curl http://httpbin.girus:8000/ip -s -o /dev/null -w "%{http_code}\n"

kubectl get policies.authentication.istio.io --all-namespaces

kubectl get meshpolicies.authentication.istio.io --all-namespaces

kubectl get destinationrules.networking.istio.io --all-namespaces -o yaml | grep "host:"

# vim meshpolicy.yaml
apiVersion: "authentication.istio.io/v1alpha1"
kind: "MeshPolicy"
metadata:
  name: "default"
spec:
  peers:
  - mtls: {}

kubectl apply -f meshpolicy.yaml

kubectl exec -ti -n strigus sleep-7d457d69b5-h8frp -- curl http://httpbin.giropops:8000/ip -s -o /dev/null -w "%{http_code}\n"

kubectl exec -ti -n giropopssleep-7d457d69b5-tw5pt -- curl http://httpbin.strigus:8000/ip -s -o /dev/null -w "%{http_code}\n"

kubectl exec -ti -n giropopssleep-7d457d69b5-tw5pt -- curl http://httpbin.girus:8000/ip -s -o /dev/null -w "%{http_code}\n"

# vim destination-rule-meshpolicy.yaml
apiVersion: "networking.istio.io/v1alpha3"
kind: "DestinationRule"
metadata:
  name: "default"
  namespace: "istio-system"
spec:
  host: "*.local"
  trafficPolicy:
    tls:
      mode: ISTIO_MUTUAL


kubectl apply -f destination-rule-meshpolicy.yaml

kubectl exec -ti -n strigus sleep-7d457d69b5-h8frp -- curl http://httpbin.giropops:8000/ip -s -o /dev/null -w "%{http_code}\n"

kubectl exec -ti -n giropopssleep-7d457d69b5-tw5pt -- curl http://httpbin.strigus:8000/ip -s -o /dev/null -w "%{http_code}\n"

kubectl exec -ti -n giropopssleep-7d457d69b5-tw5pt -- curl http://httpbin.girus:8000/ip -s -o /dev/null -w "%{http_code}\n"

# vim destination-rule-fix-girus-conn.yaml
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: "httpbin-girus"
  namespace: "girus"
spec:
  host: "httpbin.girus.svc.cluster.local"
  trafficPolicy:
    tls:
      mode: DISABLE

kubectl apply -f destination-rule-fix-girus-conn.yaml

Limpando a casa após o exercício:

kubectl delete meshpolicy default

kubectl delete destinationrules httpbin-girus -n girus

kubectl delete destinationrules api-server -n istio-system

kubectl delete destinationrules default -n istio-system

# vim policy-enable-mtls-namespace.yaml
apiVersion: "authentication.istio.io/v1alpha1"
kind: "Policy"
metadata:
  name: "default"
  namespace: "giropops"
spec:
  peers:
  - mtls: {}


kubectl apply -f policy-enable-mtls-namespace.yaml

for from in "giropops" "strigus"; do for to in "giropops" "strigus"; do kubectl exec $(kubectl get pod -l app=sleep -n ${from} -o jsonpath={.items..metadata.name}) -c sleep -n ${from} -- curl "http://httpbin.${to}:8000/ip" -s -o /dev/null -w "sleep.${from} to httpbin.${to}: %{http_code}\n"; done; done

# vim destination-rule-enable-mtls-namespace.yaml
apiVersion: "networking.istio.io/v1alpha3"
kind: "DestinationRule"
metadata:
  name: "default"
  namespace: "giropops"
spec:
  host: "*.giropops.svc.cluster.local"
  trafficPolicy:
    tls:
      mode: ISTIO_MUTUAL

kubectl apply -f destination-rule-enable-mtls-namespace.yaml

for from in "giropops" "strigus"; do for to in "giropops" "strigus"; do kubectl exec $(kubectl get pod -l app=sleep -n ${from} -o jsonpath={.items..metadata.name}) -c sleep -n ${from} -- curl "http://httpbin.${to}:8000/ip" -s -o /dev/null -w "sleep.${from} to httpbin.${to}: %{http_code}\n"; done; done

for from in "giropops" "strigus" "girus"; do for to in "giropops" "strigus" "girus"; do kubectl exec $(kubectl get pod -l app=sleep -n ${from} -o jsonpath={.items..metadata.name}) -c sleep -n ${from} -- curl "http://httpbin.${to}:8000/ip" -s -o /dev/null -w "sleep.${from} to httpbin.${to}: %{http_code}\n"; done; done

# vim policy-enable-mtls-service.yaml
apiVersion: "authentication.istio.io/v1alpha1"
kind: "Policy"
metadata:
  name: "httpbin"
  namespace: strigus
spec:
  targets:
  - name: httpbin
  peers:
  - mtls: {}


kubectl apply -f policy-enable-mtls-service.yaml


for from in "giropops" "strigus" "girus"; do for to in "giropops" "strigus" "girus"; do kubectl exec $(kubectl get pod -l app=sleep -n ${from} -o jsonpath={.items..metadata.name}) -c sleep -n ${from} -- curl "http://httpbin.${to}:8000/ip" -s -o /dev/null -w "sleep.${from} to httpbin.${to}: %{http_code}\n"; done; done


# vim destination-rule-enable-mtls-service.yaml
apiVersion: "networking.istio.io/v1alpha3"
kind: "DestinationRule"
metadata:
  name: "httpbin"
  namespace: strigus
spec:
  host: "httpbin.strigus.svc.cluster.local"
  trafficPolicy:
    tls:
      mode: ISTIO_MUTUAL


kubectl apply -f destination-rule-enable-mtls-service.yaml

for from in "giropops" "strigus" "girus"; do for to in "giropops" "strigus" "girus"; do kubectl exec $(kubectl get pod -l app=sleep -n ${from} -o jsonpath={.items..metadata.name}) -c sleep -n ${from} -- curl "http://httpbin.${to}:8000/ip" -s -o /dev/null -w "sleep.${from} to httpbin.${to}: %{http_code}\n"; done; done

# vim policy-enable-mtls-specific-service.yaml
apiVersion: "authentication.istio.io/v1alpha1"
kind: "Policy"
metadata:
  name: "httpbin"
  namespace: strigus
spec:
  targets:
  - name: httpbin
    ports:
    - number: 8080
  peers:
  - mtls: {}


kubectl  apply -f policy-enable-mtls-specific-service.yaml


# vim destination-rule-specific-service.yaml
apiVersion: "networking.istio.io/v1alpha3"
kind: "DestinationRule"
metadata:
  name: "httpbin"
  namespace: strigus
spec:
  host: httpbin.strigus.svc.cluster.local
  trafficPolicy:
    tls:
      mode: DISABLE
    portLevelSettings:
    - port:
        number: 8080
      tls:
        mode: ISTIO_MUTUAL


kubectl apply -f destination-rule-specific-service.yaml

kubectl exec -ti -n girus sleep-7d457d69b5-sr5xw -- curl http://httpbin.strigus:8000/ip -s -o /dev/null -w "%{http_code}\n"

kubectl exec -ti -n girus sleep-7d457d69b5-sr5xw -- curl http://httpbin.giropops:8000/ip -s -o /dev/null -w "%{http_code}\n"

kubectl exec $(kubectl get pod -l app=sleep -n girus -o jsonpath={.items..metadata.name}) -c sleep -n girus -- curl http://httpbin.strigus:8000/ip -s -o /dev/null -w "%{http_code}\n"

# Limpando a casa após o exercício:


kubectl delete policy default -n giropops

kubectl delete policy httpbin -n strigus

kubectl delete destinationrules default -n giropops

kubectl delete destinationrules httpbin -n strigus
```
