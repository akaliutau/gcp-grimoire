# Definitions

Anthos clusters are registered as part of a Google Cloud fleet using Connect, allowing multiple clusters to be viewed and managed together in the Anthos dashboard.

The fleet is a logical grouping of Kubernetes clusters and other resources that can be managed together.

# Microservice architecture support

Anthos Service Mesh is based on Istio, which is an open-source implementation of the service mesh infrastructure layer. Anthos Service Mesh uses sidecar proxies to enhance network security, reliability, and visibility.

Anthos Service Mesh features include:

- Fine-grained control of traffic with rich routing rules for HTTP(S), gRPC, and TCP traffic.
- Automatic metrics, logs, and traces for all HTTP traffic within a cluster, including cluster ingress and egress.
- Secure service-to-service communication with authentication and authorization based on service accounts.
- Facilitation of tasks like A/B testing and canary rollouts.

# Service Mesh

The Service Mesh dashboard allows to:

- Get an overview of all services in your mesh, providing you critical, service-level metrics on three of the four golden signals of monitoring: latency, traffic, and errors.
- Define, review, and set alerts against service level objectives (SLOs), which summarize your service's user-visible performance.
- View metric charts for individual services and deeply analyze them with filtering and breakdowns, including by response code, protocol, destination Pod, traffic source, and more.
- Get detailed information about the endpoints for each service, and see how traffic is flowing between services, and what performance looks like for each communication edge.
- Explore a service topology graph visualization that shows your mesh's services and their relationships.


# Centralized config management

Anthos Config Management integrates with Anthos clusters on-premises or in the cloud. It lets to deploy and monitor configuration changes stored in a central Git repository.

This approach leverages core Kubernetes concepts, such as Namespaces, labels, and annotations to determine how and where to apply the config changes to all of your Kubernetes clusters, no matter where they reside.

Anthos Config Management has the following benefits:

- Single source of truth, control, and management: 
Enables the use of code reviews, validation, and rollback workflows; 
Helps to avoid shadows ops due to manual changes; enables the use of CI/CD pipelines for automated testing and rollout.

- One-step deployment across all clusters: 
Anthos Config Management turns a single Git commit into multiple kubectl commands across all clusters
Rollback is simple - by reverting the change in Git. The reversion is then automatically deployed at scale.

- Rich inheritance model for applying changes: 
Using Namespaces, one can create configuration for all clusters, some clusters, some Namespaces, or even custom resources
Plus one can create a layered Namespace model that allows for configuration inheritance across the repo folder structure.

- Advanced policy enforcement and auditing with Policy Controller: 
Used to enforce security best practices across your entire environment, continuously audit your environment for configuration that violates business policies, define and deploy your own custom rules, using an expressive custom policy language to encode your unique business logic.


