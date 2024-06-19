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

1. Please following for installation of NGINX Ingress Operator

https://github.com/nginxinc/nginx-ingress-helm-operator/blob/main/docs/openshift-installation.md

2. Create SCC:

```
kubectl apply -f https://raw.githubusercontent.com/nginxinc/nginx-ingress-helm-operator/v2.2.2/resources/scc.yaml
```
3. Create secret for image-pull

Example secret for docker hub private repo
```
oc create secret docker-registry <pull-secret-name> --docker-server=https://index.docker.io/v1/ --docker-username=<user-name> --docker-password=<your-password> --docker-email=<user-email> -n nginx-ingress
oc secrets link default <pull-secret-name> --for=pull -n nginx-ingress
```

2. Install Nginx+ Ingress

<img width="1315" alt="image" src="https://github.com/bsmerja/ocp-n-ingress-cis-ingresslink/assets/49276353/03471ae8-9801-4d3c-bd2b-4cafe055aabe">

<img width="1383" alt="image" src="https://github.com/bsmerja/ocp-n-ingress-cis-ingresslink/assets/49276353/4305c75a-90a7-4832-8a8f-84895b11737c">

3. Edit yaml file and incorporate following changes

Under Spec > Controller
```
    config:
      annotations: {}
      entries:
        proxy-protocol: 'True' # This is to set proxy_protocol under listner
        real-ip-header: proxy_protocol
        set-real-ip-from: 0.0.0.0/0
    ...
    nginxplus: true
    ...
    service:
      create: true
      externalTrafficPolicy: Cluster
      extraLabels:
        app: ingresslink
      httpPort:
        enable: true
        port: 80
        targetPort: 80
      httpsPort:
        enable: true
        port: 443
        targetPort: 443
      type: ClusterIP
    ...
    serviceAccount:
      annotations: {}
      imagePullSecretName: <>
      imagePullSecretsNames: []




