# Definition

Google Kubernetes Engine is an implementation of containers as a service (CaaS) technology. Basically it's a K8s managed platform.

# Creation

This is a 3-node cluster deployed to the ```us-east-1b``` zone running on ```n1-standard-1``` machine:

```
gcloud container clusters create simple-gke-cluster --zone us-east-1b --machine-type n1-standard-1
```

Much of a cluster's configuration is immutable(cannot be changed without deleting the cluster and recreating it with a different configuration)

 The high-level constructs that make up a GKE cluster include:
 
* Container clusters == deployable units that can be created, scheduled, and managed. They are a logical collection of containers that belong to an application.
* Nodes == workers that run tasks as delegated by the master. Nodes can run one or more container clusters. Each provides an application-specific virtual host in a containerized environment.
* Cluster master  - the central control point that provides a unified view of the cluster. There is a single master node that controls multiple nodes.


GCP Container Registry (GCR) provides a secure, reliable, and performant location to store all docker images. Main advantages:

* Automatically building and pushing container images to Container Registry from your source repository
* Triggering builds by source code or tag changes in Google Cloud Source Repositories, GitHub, or Bitbucket
* Running unit tests, export artifacts, and more as part of your CI/CD pipelines

# Deployment

There are 3 options:

* kubectl run using command line parameters
* kubectl apply using YAML deployment file
* Kubernetes Engine Workloads dashboard (can generate the YAML deployment file for a workload, and consequently switch to option #2)

# Exposing GKE Services

* Cluster IP: Exposes the workload via internal IP to the cluster
* Node port: Exposes the workload via a specific port on each node within the cluster
* Load balancer: Creates a load balancer which then exposes the workload via an external IP address

# Creating/Storing secrets

```
kubectl create secret generic creds --from-literal=username=user1 --from-literal=password=pwd
kubectl create secret generic credentials --from-file ./username.txt --from-file ./password.txt
```

# Rolling Updates

1. Building a new image:

```
docker build -t gcr.io/${PROJECT_ID}/hello-world-upd:v2
```

2. Uploading updated image (either manually or via an automated process) to Google Container Registry:

```
gcloud docker -- push gcr.io/${PROJECT_ID}/hello-world-upd:v2
```

3. The actual rolling update command is quite straightforward, with all the heavy lifting being performed under the covers by GKE and Kubernetes:

```
kubectl set image deployment/hello-world hello-world=gcr.io/${PROJECT_ID}/hello-world-upd:v2
```

# Rolling back

```
kubectl rollout undo deployment/$(DEPLOYMENT_NAME) --to-revision=2
```
