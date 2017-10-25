---
title: Service Catalog
approvers:
- chenopis
---

{% capture overview %}
{% include templates/glossary/snippet.md term="service-catalog" length="long" %}

{% endcapture %}


{% capture body %}
## Example use case

An [Application Developer](/docs/reference/glossary/?user-type=true#term-application-developer) wants to use a datastore, such as MySQL, as part of their application running in a Kubernetes cluster. However, they do not want to deal with the overhead of setting one up and administrating it themselves. Fortunately, there is a cloud provider that offers MySQL databases as a *Service* via a *Service Broker*.

A *Service Broker*, as defined by the [Open Service Broker API spec](https://github.com/openservicebrokerapi/servicebroker/blob/v2.13/spec.md), is an endpoint that manages the lifecycle of a set of Services, where a *Service* is a managed software offering that can be used by an application and is typically available via HTTP REST endpoints.
{: .note}

Using Service Catalog, the application developer can browse the list of Services through the Service Broker, provision a MySQL database instance, and bind with it to get the configuration and credentials necessary for the application to utilize the database.

## Architecture

Service Catalog is built on the [Open Service Broker API](https://github.com/openservicebrokerapi/servicebroker) and consists of its own API Server and Controller, outside of the Kubernetes Core. It communicates with Service Brokers via the OSB API and serves as an intermediary for the Kubernetes API Server in order to negotiate the initial provisioning and return the information needed for the application to use the Service.

![Service Catalog Architecture](/images/docs/service-catalog-architecture.svg)

## Usage


### List

![List Services](/images/docs/service-catalog-list.svg)

1. The Catalog Operator can add Broker Resources, which identify available Service Brokers.
1. Service Catalog requests a list of Services from the Service Broker.
1. The Service Broker returns a list of Services.
1. The Service Consumer can query the ServiceClass for a list of Services.

### Provision

![Provision a Service](/images/docs/service-catalog-provision.svg)

1. The Service Consumer provisions a new instance.
1. Service Catalog requests an instance from the Service Broker.
1. The Service Broker creates a new instance of the Service.
1. The Service Consumer can check the status of the instance.
1. The Service instance is ready for use.

### Bind

![Bind to a Service](/images/docs/service-catalog-bind.svg)

1. The Service Consumer requests a binding to the instance.
1. The Service Broker returns provider-specific information, such as coordinates, credentials, configs, for Kubernetes to access the Service.
1. Binding information and credentials are delivered to the Kubernetes API Server.

### Cleanup
1. Delete binding
1. Delete instance

Credentials are delivered to users in normal Kubernetes secrets and contain information necessary to connect with the service instance.

## API Resources

* ServiceBroker - an external service broker
* ServiceClass - a service
* ServiceInstance - the instantiation of a service
* ServiceBinding - interface to use the service instance

[sample service brokers](https://github.com/openservicebrokerapi/servicebroker/blob/master/gettingStarted.md#sample-service-brokers)

*Broker Resources* identify available Service Brokers and can be added to a service catalog.

{% endcapture %}


{% capture whatsnext %}
* Explore the [service-catalog](https://github.com/kubernetes-incubator/service-catalog) project.

{% endcapture %}


{% include templates/concept.md %}
