### Entry point to a virtual service in the Istio ambient mesh

The repository provides two options for defining a K8s service as an entry point to an Istio virtual service deployed within an ambient mesh. 
1. 1. By exposing a designated, known pod, say busybox, as a dummy
2. By exposing the backend pods that are the L7 targets of the virtual service

