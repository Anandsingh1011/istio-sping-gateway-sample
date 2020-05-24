# istio-sping-gateway-sample
Simple Example of Istio Ingress using kubernetes via minikibe and spring boot service.


### Install Istio


curl -L https://istio.io/downloadIstio | sh -

cd istio-1.6.0

export PATH=$PWD/bin:$PATH

istioctl install --set addonComponents.grafana.enabled=true

istioctl install --set values.tracing.enabled=true

istioctl install --set values.tracing.ingress.enabled=true 
 
istioctl manifest apply --set values.kiali.enabled=true

kubectl label namespace default istio-injection=enabled


### Open Dashboard


minikube dashboard

istioctl dashboard kiali

istioctl dashboard jaeger



### Deploy Application 

git clone https://github.com/Anandsingh1011/istio-sping-gateway-sample.git

cd istio-sping-gateway-sample/caller-service

skaffold run # This will build and deploy application in minikube


cd istio-sping-gateway-sample/callme-service

skaffold run # This will build and deploy application in minikube



Note : To see log while deploying run below
skaffold run --tail --no-prune=false --cache-artifacts=false   


Now you can access application


### To Expose Istio-Gateway 

1. Run this command in a new terminal window to start a Minikube tunnel that sends traffic to your Istio Ingress Gateway:

minikube tunnel

2. Get IP

minikube ip

INGRESS_IP

3. Get PORT

INGRESS_PORT

kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}'


4. Run below curl

curl -v  -H 'Host: caller.example.com' http://INGRESS_IP:INGRESS_PORT/caller/ping 

5. To create load run below 


for ((i=1;i<=1000;i++)); do   curl -v  -H 'Host: caller.example.com' http://192.168.64.11:32621/caller/ping ; done


