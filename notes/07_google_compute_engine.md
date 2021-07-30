# Definitions

Google Compute Engine (GCE) allows developers to run self-managed virtual machines on Google's infrastructure. 
This includes many tools and features that make it possible to effectively deploy and manage large numbers of VMs.

Virtual machines on Compute Engine are available in a number of configurations, known as machine types . Machine types can be categorized by use case as well as scale, where use case determines relative resource allocations (for example, more memory than vCPU), and scale represents total resource allocation for that type (for example, 4 GB of RAM per 2 vCPU).


Check out the full list of available VMs:

```
gcloud compute machine-types list --filter="zone:(<ZONE>)"
gcloud compute accelerator-types list --filter="zone:(<ZONE>)"
```

