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

# Request payload for GCP
==========================

gcloud container clusters get-credentials CLUSTER_NAME --zone us-central1-c --project PROJECT_ID

gcloud auth configure-dockerc

Get IP on GCP Istio Ingress 

export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')

Port will be 80

curl -v  -H 'Host: caller.example.com' http://34.69.81.110/caller/ping

for ((i=1;i<=1000;i++)); do  curl -v  -H 'Host: caller.example.com' http://34.69.81.110/caller/ping ; done
