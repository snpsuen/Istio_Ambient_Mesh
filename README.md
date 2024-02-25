### Entry point to a virtual service in the Istio ambient mesh

The repo provides two options for defining a K8s service as an entry point to an Istio virtual service deployed within an ambient mesh. 
1. By exposing a designated, known pod, say busybox, as a dummy
2. By exposing the backend pods that are the L7 targets of the virtual service

Recall that an entry point to a virtual service is a K8s service specified for the .spec.hosts[0] field in the yaml manifest of the virtual service.
```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: virtualservice
spec:
  hosts:
    - entry-point
...
```

#### Sample use case

We work with the same use case as the one devised earlier for another repo about [Istio service mesh](https://github.com/snpsuen/Intra-K8s_Access_Istio_Service_Mesh). In essence, it consists in a frontend pod using a virtual service to perform L7 content or traffic switching between different backend services. 
1. Car catalog service
2. Truck catalog service
3. Web application
    
#### Option 1

In this option, the entry point is defined to expose a filler pod based on the well known busybox docker. It is a more transparent approach as there is no need to make any change to the K8s manifests of the backend services and workloads. Any attributes or settings related to the waypoint proxy weigh squarely on the filler pod of the entry point and nothing else. Nevertheless the downside is, the filler pod requires allocation of additional resources from the K8s cluster.

##### Deployment procedure

Assume you have read through the [quickstart guide](https://istio.io/latest/docs/ops/ambient/getting-started/) to set up an ambient mesh on a K8s cluster.

1.  Deploy the backend workloads and services.
```
kubectl apply -f https://raw.githubusercontent.com/snpsuen/Istio_Ambient_Mesh/main/Option01/manifests/car-truck-catalog-deployment-service.yaml
kubectl apply -f https://raw.githubusercontent.com/snpsuen/Istio_Ambient_Mesh/main/Option01/manifests/webapp-deployment-v4041-service.yaml
```
2.  Deploy the frontend pod and service.
```
kubectl apply -f https://github.com/snpsuen/Istio_Ambient_Mesh/raw/main/Option01/manifests/meshfront-deployment-service.yaml
```
3.  Deploy the entry point pod and service based on the busybox docker.

In addition, a service account named service-mesh is also defined so that the entry point is assigned to it.
```
kubectl apply -f https://github.com/snpsuen/Istio_Ambient_Mesh/raw/main/Option01/manifests/service-mesh-deployment-service.yaml
```







