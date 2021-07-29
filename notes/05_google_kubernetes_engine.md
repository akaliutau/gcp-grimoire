# Definition

Google Kubernetes Engine is an implementation of containers as a service (CaaS) technology.

# Creation

This is a 3-node cluster deployed to the ```us-east-1b``` zone running on ```n1-standard-1``` machine:

```
gcloud container clusters create my-gke-cluster --zone us-east-1b --machine-type n1-standard-1
```

Much of a cluster's configuration is immutable(cannot be changed without deleting the cluster and recreating it with a different configuration)

 The high-level constructs that make up a GKE cluster include:
 
* Container clusters == deployable units that can be created, scheduled, and managed. They are a logical collection of containers that belong to an application.
* Nodes == workers that run tasks as delegated by the master. Nodes can run one or more container clusters. Each provides an application-specific virtual host in a containerized environment.
* Cluster master  - the central control point that provides a unified view of the cluster. There is a single master node that controls multiple nodes.






