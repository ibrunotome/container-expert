```bash

kubectl get policies.authentication.istio.io --all-namespaces

kubectl get meshpolicy

kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml

kubectl get deployments. --all-namespaces

kubectl apply -f samples/bookinfo/networking/destination-rule-all-mtls.yaml

kubectl get destinationrules -o yaml

kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml

kubectl get deployments. --all-namespaces

kubectl get svc

kubectl port-forward svc/productpage 9080:9080 -n default --address=0.0.0.0 &

# vim samples/bookinfo/platform/kube/rbac/rbac-config-ON.yaml
kind: ClusterRbacConfig
metadata:
  name: default
spec:
  mode: 'ON_WITH_INCLUSION'
  inclusion:
    namespaces: ["default"]

kubectl apply -f samples/bookinfo/platform/kube/rbac/rbac-config-ON.yaml

# vim samples/bookinfo/platform/kube/rbac/namespace-policy.yaml
apiVersion: "rbac.istio.io/v1alpha1"
kind: ServiceRole
metadata:
  name: service-viewer
  namespace: default
spec:
  rules:
  - services: ["*"]
    methods: ["GET"]
    constraints:
    - key: "destination.labels[app]"
      values: ["productpage", "details", "reviews", "ratings"]
---
apiVersion: "rbac.istio.io/v1alpha1"
kind: ServiceRoleBinding
metadata:
  name: bind-service-viewer
  namespace: default
spec:
  subjects:
  - properties:
      source.namespace: "istio-system"
  - properties:
      source.namespace: "default"
  roleRef:
    kind: ServiceRole
    name: "service-viewer"


kubectl apply -f samples/bookinfo/platform/kube/rbac/namespace-policy.yaml

kubectl get servicerole

kubectl get servicerolebindings.rbac.istio.io

# vim samples/bookinfo/platform/kube/rbac/productpage-policy.yaml
apiVersion: "rbac.istio.io/v1alpha1"
kind: ServiceRole
metadata:
  name: productpage-viewer
  namespace: default
spec:
  rules:
  - services: ["productpage.default.svc.cluster.local"]
    methods: ["GET"]
---
apiVersion: "rbac.istio.io/v1alpha1"
kind: ServiceRoleBinding
metadata:
  name: bind-productpage-viewer
  namespace: default
spec:
  subjects:
  - user: "*"
  roleRef:
    kind: ServiceRole
    name: "productpage-viewer"

kubectl apply -f samples/bookinfo/platform/kube/rbac/details-reviews-policy.yaml

kubectl apply -f samples/bookinfo/platform/kube/rbac/ratings-policy.yaml

# vim samples/bookinfo/platform/kube/rbac/ratings-policy.yaml
apiVersion: "rbac.istio.io/v1alpha1"
kind: ServiceRole
metadata:
  name: ratings-viewer
  namespace: default
spec:
  rules:
  - services: ["ratings.default.svc.cluster.local"]
    methods: ["GET"]
---
apiVersion: "rbac.istio.io/v1alpha1"
kind: ServiceRoleBinding
metadata:
  name: bind-ratings
  namespace: default
spec:
  subjects:
  - user: "cluster.local/ns/default/sa/bookinfo-reviews"
  roleRef:
    kind: ServiceRole
    name: "ratings-viewer"

kubectl get serviceaccounts

kubectl delete -f samples/bookinfo/platform/kube/rbac/namespace-policy.yaml

kubectl delete -f samples/bookinfo/platform/kube/rbac/ratings-policy.yaml

kubectl delete -f samples/bookinfo/platform/kube/rbac/details-reviews-policy.yaml
```
