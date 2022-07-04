
# CPU allocation (services) 

By default, Cloud Run container instances are only allocated CPU during request processing, container startup and shutdown. 
You can change this behavior so CPU is always allocated and available even when there are no incoming requests. Setting the CPU to be always allocated can be useful for running short-lived background tasks and other asynchronous processing tasks.

An instance will never stay idle for more than 15 minutes after processing a request unless it is kept active using minimum instances.

# Static outbound IP address 

By default, a Cloud Run service connects to external endpoints on the internet using a dynamic IP address pool. This default is not suitable if the Cloud Run service connects to an external endpoint that requires connections originating from a static IP address, such as a database or API using an IP address-based firewall. For those connections, you must configure your Cloud Run service to route requests through a static IP address.

This can be achieved via creating a Serverless VPC Access connector `gcloud compute networks vpc-access connectors create`, then reserve a static IP address and create a Cloud NAT gateway.