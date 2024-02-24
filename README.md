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

In this option, the entry point is defined to expose a filler pod based on the well known busybox docker. It is a more transparent approach as there is no need to make any change to the K8s manifests of the backend services and workloads. Any attributes required for the waypoint proxy weigh purely on the filler pod of the entry point. Nevertheless the downside is, the filler pod requires allocation of additional resources from the K8s cluster.



