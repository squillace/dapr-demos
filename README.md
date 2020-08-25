# Dapr demos

Collection of personal Dapr demo

## Cluster Setup 

[Setup](./setup) - Deploys and configures Dapr in Kubernates:

  * Cluster - List existing, create, add node pool, delete
  * Cert - Create and installs TLS certificates 
  * Infra - Configure DNS, ingress, observability, forward ports
  * Dapr - Deploys, configures, and test Dapr API 
  * Data - Install Redis and Mongo stateful clusters

## Demos 

* Bindings
  * [Scheduling using cron](./cron-binding) - Using scheduler to execute service 
  * [Tweet stream](./tweet-provider) - Subscribing to a Twitter even stream and publishing to a pub/sub topic
  * [State change handler](./state-change-handler) - RethinkDB state changes streamed into topic
* Eventing
  * [gRPC event subscriber](./grpc-event-subscriber) - Subscribing to topic and processing its events using gRPC service
  * [HTTP event subscriber](./http-event-subscriber) - Subscribing to topic and processing its events using HTTP service
* Services 
  * [gRPC service](./grpc-service) - gRPC service invocation example
  * [Sentiment Scorer](./sentiment-scorer) - Sentiment scoring serving backed by Azure Cognitive Service 
* Integrations
  * [Dapr API in ACI](./dapr-aci) - Dapr components as microservices 
  * [Dapr as component API](./component-api) - Zero app Dapr instance as component API server 
* Solutions
  * [Order cancellation](./order-cancellation) - multiple Dapr service integrations with observability
  * [Pipeline](./pipeline) - Demos combining Twitter binding, Sentiment scoring, Multi Pub/Sub Processor, and WebSocket Viewer app
  * [Fan-out](./fan-out) - Single message source (Event Hub) "broadcasted" to multiple, configurable targets (e.g. Redis PubSub, HTTP, gRPC)

## Disclaimer

This is my personal project and it does not represent my employer. I take no responsibility for issues caused by this code. I do my best to ensure that everything works, but if something goes wrong, my apologies is all you will get.

## License

This software is released under the [MIT](./LICENSE)
