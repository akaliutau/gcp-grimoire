# Network Availability

Data within the GCP can be transmitted among regions using the public Internet or Google’s internal network. The latter is available as the Premium Network Tier, which costs more than the Standard Network Tier, which uses the public Internet. The internal Google network is designed for high availability and low latency, so the Premium Tier should be considered if global network availability is a concern.

# Reserved IP addresses in IPv4 subnets
Every subnet has four reserved IP addresses in its primary IP range. There are no reserved IP addresses in the secondary IP ranges.


|IP address	                | Description	                                      | Example                     |
|---------------------------|-----------------------------------------------------|-----------------------------|
|Network	                |First address in the primary IP range for the subnet | 10.1.2.0 in 10.1.2.0/24     |
|Default gateway	        |Second address in the primary IP range for the subnet| 10.1.2.1 in 10.1.2.0/24     |
|Second-to-last address	    |Second-to-last address in the primary IP range       | 10.1.2.254 in 10.1.2.0/24   |
|Broadcast	                |Last address in the primary IP range for the subnet  |	10.1.2.255 in 10.1.2.0/24   |

# Google Private access

Google Cloud provides several private access options that let VMs in VPC networks reach supported APIs and services without requiring an external IP address.

- Connect to Google APIs
- Connect to services
- Connect from serverless Google services to VPC networks

# Cloud Router

Cloud Router is a fully distributed and managed Google Cloud service that uses the [Border Gateway Protocol (BGP)](https://en.wikipedia.org/wiki/Border_Gateway_Protocol) to advertise IP address ranges.

Cloud Router provides BGP services for:

- Dedicated Interconnect
- Partner Interconnect
- Cloud VPN, specifically HA VPN
- Router appliance (part of Network Connectivity Center)


# Cloud NAT

Cloud NAT (network address translation) lets certain resources without external IP addresses create outbound connections to the internet. Cloud NAT configures the Andromeda software that powers VPC network so that it provides source network address translation (source NAT or SNAT) for VMs without external IP addresses. Note, this is _not_ a proxy - each VM is programmed by GCP to NAT using assigned ports  
 
Cloud NAT provides outgoing connectivity for:

- Compute Engine VM instances without external IP addresses
- Private GKE clusters
- Cloud Run instances via Serverless VPC Access
- Cloud Functions instances via Serverless VPC Access
- App Engine standard environment instances via Serverless VPC Access

# VPC Peering

Google Cloud VPC Network Peering allows internal IP address connectivity across two Virtual Private Cloud (VPC) networks regardless of whether they belong to the same project/organization.
VPC Network Peering allows to connect VPC networks so that workloads in different VPC networks can communicate internally. Traffic stays within Google's network.


# Shared VPC

Shared VPC allows an organization to connect resources from multiple projects to a common VPC network, so that they can communicate with each other securely and efficiently using internal IPs from that network. In Shared VPC, there is a host project and one can attach one or more other service projects to it. The VPC networks in the host project are called **Shared VPC networks**. Eligible resources from service projects can use subnets in the Shared VPC network.
NOTE: Participating host and service projects cannot belong to different organizations.

# Best practices for firewall rules

- Implement least-privilege principles. Block all traffic by default and only allow the specific traffic you need. This includes limiting the rule to just the protocols and ports you need.
- Use hierarchical firewall policy rules to block traffic that should never be allowed at an organization or folder level.
- For "allow" rules, restrict them to specific VMs by specifying the service account of the VMs.
- If you need to create rules based on IP addresses, try to minimize the number of rules (use ranges rather than separate rules)
- Turn on Firewall Rules Logging and use Firewall Insights to verify that firewall rules are being used in the intended way (can incur additional costs)

# Private Service Connect

Private Service Connect lets you connect to service producers using endpoints with internal IP addresses in your VPC network: instead of sending API requests to the publicly available IP addresses for service endpoints (f.e. `storage.googleapis.com`, one can send the requests to the internal IP address of a Private Service Connect endpoint)

Those endpoints then will be available from any VM instance inside VPC network:

```
curl -v ENDPOINT_IP/generate_204
```

# Private Google Access

VM instances that only have internal IP addresses (no external IP addresses) can use Private Google Access. They can reach the external IP addresses of Google APIs and services. The source IP address of the packet can be the primary internal IP address of the network interface or an address in an alias IP range that is assigned to the interface. If you disable Private Google Access, the VM instances can no longer reach Google APIs and services; they can only send traffic within the VPC network.

This can be achieved using command for appropriate subnet: `gcloud compute networks subnets update` with `--enable-private-ip-google-access` option

# Load balancers

Proxy load balancers terminate incoming client connections and open new connections from the load balancer to the backends. 
Pass-through load balancers do not terminate client connections. Instead, load-balanced packets are received by backend VMs with the packet's source, destination, and, if applicable, port information unchanged. Connections are then terminated by the backend VMs. Responses from the backend VMs go directly to the clients, not back through the load balancer.


HTTP and HTTPS traffic:
- Global external HTTP(S) load balancer
- Global external HTTP(S) load balancer (classic)
- Internal HTTP(S) Load Balancing

TCP/UDP traffic: 
- TCP Proxy Load Balancing
- External TCP/UDP Network Load Balancing
- Internal TCP/UDP Load Balancing

SSL traffic:
- SSL Proxy Load Balancing

# Cloud Endpoints

Endpoints is an API management system that helps you secure, monitor, analyze, and set quotas on your APIs using the same infrastructure Google uses for its own APIs. After you deploy your API to Endpoints, you can use the Cloud Endpoints Portal to create a developer portal, a website that users of your API can access to view documentation and interact with your API.

There are three options, depending on where your API is hosted and the type of communications protocol your API uses:

- Cloud Endpoints for OpenAPI
- Cloud Endpoints for gRPC
- Cloud Endpoints Frameworks for the App Engine standard environment

# Carrier Peering

Carrier Peering enables you to access Google applications, such as Google Workspace, by using a service provider to obtain enterprise-grade network services that connect your infrastructure to Google. When connecting to Google through a service provider, you can get connections with higher availability and lower latency, using one or more links.

NOTE: this does not give private IP addressing across the connection; note also that  an additional Internet connection will not provide RFC1918 communications by itself.

# VPC Service control

VPC Service Controls improves your ability to mitigate the risk of data exfiltration from Google Cloud services such as Cloud Storage and BigQuery. You can use VPC Service Controls to create perimeters that protect the resources and data of services that you explicitly specify.

Security benefits of VPC Service Controls:

- Access from unauthorized networks using stolen credentials: By allowing private access only from authorized VPC networks, VPC Service Controls protects against theft of OAuth credentials or service account credentials.

- Data exfiltration by malicious insiders or compromised code: VPC Service Controls complements network egress controls by preventing clients within those networks from accessing the resources of Google-managed services outside the perimeter. VPC Service Controls also prevents reading data from or copying data to a resource outside the perimeter. VPC Service Controls prevents service operations such as a `gsutil cp` command copying to a public Cloud Storage bucket or a `bq mk` command copying to a permanent external BigQuery table.

Google Cloud also provides a restricted virtual IP that is used integrated with VPC Service Controls. The restricted VIP also allows requests to be made to services supported by VPC Service Controls without exposing those requests to the internet.

- Public exposure of private data caused by misconfigured IAM policies: VPC Service Controls provides an extra layer of security by denying access from unauthorized networks, even if the data is exposed by misconfigured IAM policies.

- Monitoring access to services: Use VPC Service Controls in dry run mode to monitor requests to protected services without preventing access and to understand traffic requests to your projects.


VPC Service Controls lets to:

- Isolate Google Cloud resources and VPC networks into service perimeters
- Extend perimeters to on-premises networks to authorized VPN or Cloud Interconnect
- Control access to Google Cloud resources from the internet
- Protect data exchange across perimeters and organizations by using ingress and egress rules
- Allow context-aware access to resources based on client attributes by using ingress rules
 
