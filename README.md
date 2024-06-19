# NGINX+ Ingress Controller and IngressLink on OpenShift

1. Set up Docker for F5 Container Registry

Please following:
https://docs.nginx.com/nginx-ingress-controller/installation/nic-images/pulling-ingress-controller-image/#set-up-docker-for-f5-container-registry

2. Pull the image
```
docker pull private-registry.nginx.com/nginx-ic/nginx-plus-ingress:<version-tag>
```
3. Tage and Push to your private registry

```
docker tag private-registry.nginx.com/nginx-ic/nginx-plus-ingress:<version-tag> <my-docker-registry>/nginx-plus-ingress:<version-tag>
docker push <my-docker-registry>/nginx-plus-ingress:<version-tag>
```

# Installation of NGINX Operator and NGINX+ Ingress Controller in an OpenShift cluster using the OLM

1. Please following:

https://github.com/nginxinc/nginx-ingress-helm-operator/blob/main/docs/openshift-installation.md

2. Install Nginx+ Ingress

<img width="1315" alt="image" src="https://github.com/bsmerja/ocp-n-ingress-cis-ingresslink/assets/49276353/03471ae8-9801-4d3c-bd2b-4cafe055aabe">


