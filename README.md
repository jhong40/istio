# istio

```
    2  curl -L https://istio.io/downloadIstio | sh -
    6  cd istio-1.12.0/
   12  export PATH=$PWD/bin:$PATH
   13  istioctl -h
   14  istioctl verify-install
   16  istioctl install --set profile=demo -y
   17  istioctl verify-install



```
