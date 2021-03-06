# Dapr Cluster Setup

An opinionated Kubernetes clusters Dapr deployment:

* Latest version of Dapr
* Metrics Monitoring
  * [Prometheus](https://prometheus.io/) for metrics aggregation
  * [Grafana](https://grafana.com/) for metrics visualization with Dapr monitoring dashboards
* Log Management
  * [Fluentd](https://www.fluentd.org/) for log collection and forwarding
  * [Elasticsearch](https://www.elastic.co/) for log aggregation and query execution
  * [Kibana](https://www.elastic.co/products/kibana) for full-text log query and visualization
* Distributed Tracing
  * [Jaeger](https://www.jaegertracing.io/) for capturing traces, latency and dependency trace viewing
* Ingress and TLS termination
  * [ngnx](https://nginx.org/en/) for ingress controller and TLS to service mapping 
  * [letsencrypt](https://letsencrypt.org/) as certificate provider
  
## Prerequisites

* 1.15+ Kubernates cluster (see [Create Cluster](#create-cluster) section below if you don't already have one)
* CLIs locally on the machine where you will be running this setup:
  * [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) to do k8s stuff (`brew install kubectl`)
  * [Helm 3](https://helm.sh/docs/intro/install/) to install Dapr and its dependencies (`brew install helm`)
  * [certbot](https://certbot.eff.org/lets-encrypt/osx-other.html) to generate wildcard cert (`brew install certbot`)

## Deployment 

* Update [Makefile](./Makefile) variables
* Run:
  * `make cluster` (optional, otherwise use one selected in your kubectol context)
  * `make certs` (optional, to create TLS certs if you don't have them)
  * `make dapr` to install Dapr
  * `make keda` (optional, to install Keda for autoscaling)
  * `make observe` to install observability stack (logging, metric, tracing)
  * `make observe-config` to configure Dapr Grafana dashboards and Kibana indexes
  * `make dns` to configure DNS
  * `make test` to test deployment 
  * `make ports` (optional) to forward observability dashboards ports
  * `make redis` (optional) to install Redis into the cluster 
  * `make mongo` (optional) to install Mongo into the cluster 
  * `make kafka` (optional) to install Kafka into the cluster 

## Observability

To get access to the Kibana, Grafana, Zipkin dashboards run:

```shell
make ports
```

Once ports are forwarded, you can access these dashboards using: 

* kibana - http://localhost:5601
* grafana - http://localhost:8888
* zipkin - http://localhost:9411

To stop port forwarding run 

```shell
make portstop
```

## Create Cluster

If you don't already have a Kubernates cluster, you can create one in AKS by following these steps

* Update [Makefile](./Makefile) to set:
  * `CLUSTER_NAME` - Name of the cluster you want to create 
  * `NODE_COUNT` - NUmber of nodes in the cluster default pool
  * `NODE_TYPE` - VM type used for the nodes in default pool 
* Run `make cluster`

> Note, this assumes your default Azure resource group and location are already defined. If not, run

```shell
az account set --subscription <id or name>
az configure --defaults location=<preferred location> group=<preferred resource group>
```

## Other Commands 

```shell
$ make help
clusterlist                    List all your AKS clusters in the default resource group
cluster                        Create AKS cluster (make cluster
nodepool                       Add new node pool to the existing cluster
certs                          Create wildcard TLS certificates using letsencrypt
dapr                           Install and configures Dapr
keda                           Install and configures Keda
observe                        Install observability stack
observe-config                 Configure observability stack
ingress                        Install and configures Ngnx ingress, configure SSL termination, Dapr API auth
dns                            Check DNS resolution for cluster IP
test                           Test deployment and execute Dapr API health checks
token                          Print Dapr API token
pass                           Print Grafana admin password
ports                          Forward observability ports
reload                         Reloads API to pickup new components
redis                          Install Redis into the cluster
mongo                          Install Mongo into the cluster
kafka                          Install Kafka into the cluster
portstop                       Stop previously forwarded observability ports
cleanup                        Delete previously created AKS cluster (make cleanup CLUSTER_NAME=demo)
help                           Display available commands
```

## Cleanup

To lists previously created clusters run 

```shell
make clusterlist
```

To delete any of the previously created clusters run 

> No worries, there will be a prompt to confirm before deleting

```shell
make cleanup CLUSTER_NAME=name
```

## Disclaimer

This is my personal project and it does not represent my employer. While I do my best to ensure that everything works, I take no responsibility for issues caused by this code.

## License

This software is released under the [MIT](../LICENSE)