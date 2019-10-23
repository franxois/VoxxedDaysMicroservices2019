# k8s

3 pods pattern:

- sidecar
- adaptator
- ambassador

https://matthewpalmer.net/kubernetes-app-developer/articles/multi-container-pod-design-patterns.html

## Configuration

### Pod

Ressources always have `ApiVersion` and `kind`
Always metadata with at least a name

good practice to define `request` and `limits`

OutOfMemory killer kill container if they overpassed memory limit

### Deployment

selector link deployment and pods

    metadata
      labels:
        XXX: toto

    selector:
      matchLabels:
        XXX: toto

Only one value by key

### services

type
ClusterIP
NodePort : give access to service for this port on any node IP
LoadBalancer : can't use it on promise cluster. Usually used with managed cluster provider

### Ingress

### other resources

batch resources
cron job batch job schedules
configuration & secrets

## Rolling-upgrade

## Let's sails with Kubernetes

https://sfeir.github.io/kubernetes-istio-workshop/kubernetes-istio-workshop/1.0.0/index.html

for liveness of gRPC services, use sidecar

You can create config map from file

kubectl create configmap nginx-proxy-conf --from-file=manifests/app/nginx/proxy.conf

Ajouter un label
kubectl label pods secure-monolith 'secure=enabled'
retirer un label
kubectl label pods secure-monolith secure-

Hint : see endpoint field in `describe` to debug label matching issue.

`kubectl rollout pause deployment hello` to pause deployment
`kubectl rollout undo deployment hello` do undo

# istio

With microservice comes new problems. Increase complexity, network latencies

Sidecar pattern

Service mesh
"Control Plane" is the single entry point used by operator

Envoy proxy is like nginx proxy but much better

Envoy : reload config without restart

"Data Plane" is where the application live

Service mesh = data plane + control plane

Istio is a service mesh, fully open source

You can deploy your app into Istio without any changes

In Isto, side car proxy captures all the traffic transparently, then send metrics

Control Plane :

- mixer
- pilot
- citadel
- gallery

Istio, mutual tls for free
We can add plugins to Mixer

Fundamental concepts
23 new concepts, most important:

- Virtual services
- Destination rule
- Gateway

Virtual Service define how requests are routed to a destination. Destination Rule define destination subsets

VirtualService can match using regexp for values in http header
Put more specific match in front of rules

You could have sticky session reading http cookie

`outlierDetection` use to circuit breaker. It uses passive health checking , you define error on delay or httpStatus. This eject instance but don't kill it

To have mTLS use `Policy` and `DestinationRule`

"Gateway" is the istio ingress. Gateway linked to a VirtualService

Gateway is a ingress implementation that uses Envoy. Envoy handle tcp load balancer

`jaeger UI` tools to see traces

`Kiali` can draw service mesh automaticaly, show real time traffic

[Workshop]

Istio detect automaticly failing service, that's why the tutorial doesn't work

# Side notes

Tutorial uses https://sfeir.github.io/kubernetes-istio-workshop/kubernetes-istio-workshop/1.0.0/index.html \
Git repository https://github.com/Sfeir/kubernetes-istio-workshop \
This is made using https://antora.org/ and AsciiDoc
