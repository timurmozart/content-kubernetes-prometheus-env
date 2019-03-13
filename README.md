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

__kubectl apply -f namespace.yml__
 
File called clusterRole.yml. This will be used to set up the cluster's roles.
clusterRole.yml:

Apply the cluster roles to the Kubernetes cluster:

__kubectl apply -f clusterRole.yml__

File called config-map.yml. Kubernetes will use this file to manage the prometheus.yml configuration file.
config-map.yml:

Apply the ConfigMap:

__kubectl apply -f prometheus-config-map.yml__

File called prometheus-deployment.yml. This file will be used to create the Prometheus deployment; which will include the pods, replica sets and volumes.
prometheus-deployment.yml

Deploy the Prometheus environment:

__kubectl apply -f prometheus-deployment.yml__

Finally, we will finish off the Prometheus environment by creating a server to make publicly accessible. Create prometheus-service.yml.
prometheus-service.yml:

__kubectl apply -f prometheus-service.yml__

Create the clusterRole.yml file to set up access so Prometheus can access metrics using Service Discovery.
clusterRole.yml:

__kubectl apply -f clusterRole.yml__

Crate the Kube State Metrics pod to get access to metrics on the Kubernetes API:
kube-state-metrics.yml:

__kubectl apply -f kube-state-metrics.yml__

Access the prometheus service with the  NodePort you specified in manifest: 

Deploy Grafana:

__kubectl apply -f grafana-deployment.yml__

Create the Grafana service:

__kubectl apply -f grafana-service.yml__


NodeExporter

Repeat these steps on both your master and worker nodes.

Create the Prometheus user:

__adduser prometheus__
Download Node Exporter:
__cd /home/prometheus__
__curl -LO "https://github.com/prometheus/node_exporter/releases/download/v0.16.0/node_exporter-0.16.0.linux-amd64.tar.gz"__
__tar -xvzf node_exporter-0.16.0.linux-amd64.tar.gz__
__mv node_exporter-0.16.0.linux-amd64 node_exporter__
__cd node_exporter__
__chown prometheus:prometheus node_exporter__
__vi /etc/systemd/system/node_exporter.service__

/etc/systemd/system/node_exporter.service:

[Unit]
Description=Node Exporter

[Service]
User=prometheus
ExecStart=/home/prometheus/node_exporter/node_exporter

[Install]
WantedBy=default.target
Reload systemd:
__systemctl daemon-reload__
Enable the node_exporter service:
__systemctl enable node_exporter.service__
Start the node_exporter service:
__systemctl start node_exporter.service__
Add the following to config-map.yaml:
      - job_name: 'node_exporter'
        static_configs:
        - targets: ['<KUBER_MASTER_PRIVATE_IP>:9100', <UBER_WORKER_PRIVATE_IP>:9100]


Repeat these steps on both your master and worker nodes.

Create the Prometheus user:

adduser prometheus
Download Node Exporter:

cd /home/prometheus
curl -LO "https://github.com/prometheus/node_exporter/releases/download/v0.16.0/node_exporter-0.16.0.linux-amd64.tar.gz"
tar -xvzf node_exporter-0.16.0.linux-amd64.tar.gz
mv node_exporter-0.16.0.linux-amd64 node_exporter
cd node_exporter
chown prometheus:prometheus node_exporter
vi /etc/systemd/system/node_exporter.service
/etc/systemd/system/node_exporter.service:

[Unit]
Description=Node Exporter

[Service]
User=prometheus
ExecStart=/home/prometheus/node_exporter/node_exporter

[Install]
WantedBy=default.target
Reload systemd:

systemctl daemon-reload
Enable the node_exporter service:

systemctl enable node_exporter.service
Start the node_exporter service:

systemctl start node_exporter.service
Add the following to config-map.yaml:

      - job_name: 'node_exporter'
        static_configs:
        - targets: ['<KUBER_MASTER_PRIVATE_IP>:9100', <UBER_WORKER_PRIVATE_IP>:9100]



