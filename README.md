# content-kubernetes-prometheus-env
Monitoring Kubernetes With Prometheus

With the help of this repository, we will set up Prometheus on the Kubernetes cluster. We will be creating:

A metrics namespace for our environment to live in
A ClusterRole to give Prometheus access to targets using Service Discovery
A ConfigMap map that will be used to generate the Prometheus config file
A Prometheus Deployment and Service
Kube State Metrics to get access to metrics on the Kubernetes API
You can clone the YAML files form Github.

File namespace.yml. This file will be used to create the monitoring namespace.
namespace.yml

Apply the namesapce:

 kubectl apply -f namespace.yml
 
File called clusterRole.yml. This will be used to set up the cluster's roles.
clusterRole.yml:

Apply the cluster roles to the Kubernetes cluster:

kubectl apply -f clusterRole.yml

File called config-map.yml. Kubernetes will use this file to manage the prometheus.yml configuration file.
config-map.yml:

Apply the ConfigMap:

kubectl apply -f config-map.yml
File called prometheus-deployment.yml. This file will be used to create the Prometheus deployment; which will include the pods, replica sets and volumes.
prometheus-deployment.yml

Deploy the Prometheus environment:

kubectl apply -f prometheus-deployment.yml

Finally, we will finish off the Prometheus environment by creating a server to make publicly accessible. Create prometheus-service.yml.
prometheus-service.yml:

kubectl apply -f prometheus-service.yml

Create the clusterRole.yml file to set up access so Prometheus can access metrics using Service Discovery.
clusterRole.yml:

kubectl apply -f prometheus-service.yml

Crate the Kube State Metrics pod to get access to metrics on the Kubernetes API:
kube-state-metrics.yml:

kubectl apply -f prometheus-service.yml

Access the prometheus service with the  NodePort you specified in manifest: 
