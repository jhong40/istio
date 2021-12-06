# istio

```
# install istio
curl -L https://istio.io/downloadIstio | sh -
cd istio-1.12.0/
export PATH=$PWD/bin:$PATH
istioctl install --set profile=demo -y
kubectl label namespace default istio-injection=enabled

istioctl -h
istioctl verify-install

# kiali
kubectl apply -f samples/addons
kubectl rollout status deployment/kiali -n istio-system
kubectl -n istio-system get svc kiali

# install sampleapp
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml

# verify the app
kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -sS productpage:9080/productpage | grep -o "<title>.*</title>"

# gateway + virtualservice
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
istioctl analyze

export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')
export INGRESS_HOST=$(minikube ip)

export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
echo "http://$GATEWAY_URL/productpage"

curl 172.17.0.54/productpage 2>&1 | grep "color"



```
