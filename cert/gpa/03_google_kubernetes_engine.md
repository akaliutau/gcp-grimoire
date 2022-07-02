# General notes

`gcloud` commands are used to manipulate a K8s cluster at GCP.
These tasks include creating, updating, and deleting clusters, adding or removing nodes, and controlling who can access the cluster using Identity and Access Management.

`kubectl` - to work with apps in this cluster or update kubernetes-related settings, i.e. to control the cluster's internal behavior


# Creating a zonal/regional/auto/private cluster

A single-zone cluster has a single control plane running in one zone. This control plane manages workloads on nodes running in the same zone.
In a regional cluster the cluster's control plane is replicated across multiple zones in a given region.

In both cases use `gcloud container clusters create`

The Autopilot mode of operation is a hands-off Kubernetes experience that lets Google to take care of node management and infrastructure via `gcloud container clusters create-auto`

By default, Autopilot clusters are public. If you created a private Autopilot cluster, these nodes do not have external IP addresses. To make outbound internet connections from your cluster (f.e. download data from internet), you must configure Cloud NAT, which lets private clusters send outbound packets to the internet and receive any corresponding established inbound response packets. 

```
gcloud compute routers create
gcloud compute routers nats create
```

In a private cluster, nodes only have internal IP addresses, which means that nodes and Pods are isolated from the internet by default. Internal IP addresses for nodes come from the primary IP address range of the subnet you choose for the cluster. Pod IP addresses and Service IP addresses come from two subnet secondary IP address ranges.
The Node, Pod, and Services IP address ranges **must all be unique**, i.e. cannot create a subnet whose primary and secondary IP address ranges overlap.


The final operation is to get access to the cluster, as gcloud is not designed to do K8s devops, this command configures (locally installed) `kubectl` to use the cluster

```
gcloud container clusters get-credentials
```

A node pool is a group of nodes within a cluster that all have the same configuration. Node pools use a NodeConfig specification. Each node in the pool has a Kubernetes node label, cloud.google.com/gke-nodepool, which has the node pool's name as its value.

When you create a cluster, the number of nodes and type of nodes that you specify are used to create the first node pool of the cluster. Then, you can add additional node pools of different sizes and types to your cluster. All nodes in any given node pool are identical to one another.

For example, you might create a node pool in your cluster with local SSDs, a minimum CPU platform, Spot VMs, a specific node image, different machine types, or a more efficient virtual network interface.


# Concepts overview

## Addressing

- ClusterIP: The IP address assigned to a Service, aka "Cluster IP". This address is stable for the lifetime of the Service
- Pod IP: The (ephemeral) IP address assigned to a given Pod
- Node IP: The IP address assigned to a given node

Kubernetes manages connectivity among Pods and Services using the kube-proxy component, which is a DaemonSet that deploys a Pod on each node by default.

- External load balancers manage traffic coming from outside the cluster and outside your Google Cloud VPC network. They use forwarding rules associated with the Google Cloud network to route traffic to a Kubernetes node.
- Internal load balancers manage traffic coming from within the same VPC network. Like external load balancers, they use forwarding rules associated with the Google Cloud network to route traffic to a Kubernetes node.
- HTTP(S) load balancers are specialized external load balancers used for HTTP(S) traffic. They use an `Ingress` K8s object rather than a forwarding rule to route traffic to a Kubernetes node; useful to map hostnames and URL paths to Services within the cluster 

## Services

The idea of a Service is to group a set of Pod endpoints into a single resource. You can configure various ways to access the grouping. By default, you get a stable cluster IP address that clients inside the cluster can use to contact Pods in the Service. A client sends a request to the stable IP address, and the request is routed to one of the Pods in the Service.

There are five types of Services:

- ClusterIP (default): Internal clients send requests to a stable internal IP address.
- NodePort: Clients send requests to the IP address of a node on one or more nodePort values that are specified by the Service.
- LoadBalancer: Clients send requests to the IP address of a network load balancer.
- ExternalName: Internal clients use the DNS name of a Service as an alias for an external DNS name.
- Headless: You can use a headless service when you want a Pod grouping, but don't need a stable IP address.

## Ingress

In GKE, an `Ingress` object defines rules for routing HTTP(S) traffic to applications running in a cluster. An Ingress object is associated with one or more Service objects, each of which is associated with a set of Pods. 


There are two types of GKE Ingress objects:

- Ingress for External HTTP(S) Load Balancing - deployed globally across Google's edge network as a managed and scalable pool of load balancing resources
- Ingress for Internal HTTP(S) Load Balancing - powered by Envoy proxy systems outside of GKE cluster, but within your VPC network.

## Node pool

A node pool is a group of nodes within a cluster that all have the same configuration. Node pools use a NodeConfig specification. Each node in the pool has a Kubernetes node label = `cloud.google.com/gke-nodepool`

## Node taints

A node taint lets to mark a node so that the scheduler avoids or prevents using it for certain Pods. A complementary feature - tolerations - lets to designate Pods that can be used on "tainted" nodes.




# Best practices for building containers 

https://cloud.google.com/kubernetes-engine/docs/best-practices

- Package a single app per container: Because a container is designed to have the same lifecycle as the app it hosts, each of your containers should contain only one app.
- Properly handle PID 1, signal handling
- Optimize for the Docker build cache: add lines with params at the end of Docker file
- Remove unnecessary tools: 
- Build the smallest image possible (https://cloudplatform.googleblog.com/2018/04/Kubernetes-best-practices-how-and-why-to-build-small-container-images.html)
- Scan images for vulnerabilities
- Properly tag your images

# Best practices for creating scalable clusters

- Preparing for availability: choosing a regional or zonal control plane (Use regional clusters for production workloads clusters as they offer higher availability than zonal clusters, and zonal if availability does matter)
- Cluster networking: use a VPC-native cluster and Private Cluster
- Cluster load balancing
- DNS: Service discovery in GKE is provided through kube-dns. For large-scale clusters or high DNS request load, enable NodeLocal DNS


# Best practices for designing cluster

- Establish a folder and project hierarchy.
- Assign roles using IAM.
- Centralize network control with Shared VPCs.
- Create one cluster admin project per cluster.
- Make clusters private.
- Ensure the control plane for the cluster is regional.
- Ensure nodes in your cluster span at least three zones.
- Autoscale cluster nodes and resources.
- Schedule maintenance windows for off-peak hours.
- Set up HTTP(S) Load Balancing with Ingress. 

# Best practices for security in clusters

- Control Pod communication with network policies: create network policies based on your tenants' requirements.
- Run workloads with GKE Sandbox: is based on gVisor, an open source container sandboxing project, and provides additional isolation for multi-tenant workloads by adding an extra layer between your containers and host OS.
- Create Pod security policies.
- Use Workload Identity to grant access to Google Cloud services.
- Restrict network access to the control plane. 

Note about security: Container runtimes often run as a privileged user on the node and have access to most system calls into the host kernel

# Tenant provisioning

- Create tenant projects.
- Use RBAC to refine tenant access: and use Google Groups to bind permissions (via RoleBinding object)
- Create namespaces for isolation between tenants.
- Enforce resource quotas (via ResourceQuota object)

# Best practices for GKE networking

- Use VPC-native clusters: VPC-native cluster use alias IP address ranges on GKE nodes and scale more easily than routes-based clusters.
- Use Shared VPC networks: a network admin can create subnets and share them with particular principals; one can then create GKE clusters in service projects in those subnets
- Choose a private cluster type.
- Minimize the cluster control plane exposure.
- Authorize access to the control plane.
- Allow control plane connectivity.
- Deploy proxies for control plane access from peered networks.
- Restrict cluster traffic using network policies: GKE network policies are configured through the Kubernetes Network Policy API (`gcloud container clusters create` option `--enable-network-policy`)
- Enable Google Cloud Armor security policies for Ingress.
- Use Identity-Aware Proxy to provide authentication for applications with IAM users.
- Use organization policy constraints to further enhance security.
- Enable NodeLocal DNSCache:  for more consistent DNS query lookup times and improved cluster scale
- Use Cloud NAT for internet access from private clusters: By default, private clusters don't have internet access. In order to allow Pods to reach the internet, enable Cloud NAT for each region. At a minimum, enable Cloud NAT for the primary and secondary ranges in the GKE subnet.
- Use Private Google Access for access to Google services.
- Use container-native load balancing:  allows far fewer network hops, lower latency, and more exact traffic distribution - implemented via Network Endpoint Groups (NEG), which hides Pods behind Service
- Choose the correct GKE resource to expose your application.
- Create health checks based on BackendConfig: BackendConfig is an equivalent of HealthCheck object at GCE
- Use local traffic policy to preserve original IP addresses.
- Use Workload Identity to authenticate your workloads towards Google APIs.
- Use IAM for GKE permissions to control policies in Shared VPC networks.
- Use regional clusters and distribute your workloads for high availability.
- Use Cloud Operations for GKE and network policy logging.

# Best practices for operating containers 

- Use the native logging mechanisms of containers
- Ensure that containers are stateless and immutable (use an object store such as Cloud Storage or Persistent Disk)
- Avoid privileged containers (Give specific capabilities to the container through the securityContext option, also privileged containers can be forbidden by using Policy Controller)
- Make the application easy to monitor
- Expose the health of application
- Carefully choose the image version

# Best practices for running cost-optimized GKE cluster

- Fine-tune GKE autoscaling
- Choose the right machine type


# Best practices for upgrading clusters

- Set up multiple environments	
- Enroll clusters in release channels	
- Create a continuous upgrade strategy	
- Reduce disruption to existing workloads	

