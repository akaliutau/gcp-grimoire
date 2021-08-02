# Definitions

Google Compute Engine (GCE) allows developers to run self-managed virtual machines on Google's infrastructure. 
This includes many tools and features that make it possible to effectively deploy and manage large numbers of VMs.

Virtual machines on Compute Engine are available in a number of configurations, known as machine types . Machine types can be categorized by use case as well as scale, where use case determines relative resource allocations (for example, more memory than vCPU), and scale represents total resource allocation for that type (for example, 4 GB of RAM per 2 vCPU).


Check out the full list of available VMs:

```
gcloud compute machine-types list --filter="zone:(<ZONE>)"
gcloud compute accelerator-types list --filter="zone:(<ZONE>)"
```


# Creating instances

## Through Cloud Console

Navigate to Navigation menu | Compute Engine | VM instances . If this is your first instance, click on the Create button.

Provide the following minimum set of parameters:

```
name: gce-sample-app
zone: us-east1-b
machine type: f1-micro
```


# Accessing instances via SSH

To establish an SSH connection to a running instance (as configured previously) from the Cloud Shell or a developer machine, run the following command:

```
gcloud compute ssh gce-sample-app --zone=us-east1-b
```

For the first time gcloud will generate a __new local SSH key__ on your behalf, and upload the public key to the Compute Engine metadata server. 
This SSH key will be available to all instances through the metadata server.

SSH access makes it possible to execute commands on remote machines as part of a script, f.e.:

```
gcloud compute ssh gce-sample-app --zone=us-east1-b -- cat /proc/cpuinfo
```

# Accessing instances via SCP

```
# Create a file to copy to the remote machine
echo “Testing SCP on Compute Engine” > test.txt
# Copy the file to the gce-sample-app instance using SCP
gcloud compute scp --zone=us-east1-b test.txt gce-sample-app:~/test.txt
# Print the remote file
gcloud compute ssh gce-sample-app --zone=us-east1-b -- cat ~/test.txt
```

# Metadata server

Compute Engine instances are able to query metadata by making API calls to the URL at http://metadata.google.internal/computeMetadata/v1/. This URL resolves internally to the instance and requests are fulfilled without ever leaving the physical machine that the instance is running on. By fulfilling requests locally, the metadata server can provide a high level of security, making the metadata server a valid option for providing instances with sensitive information.

All project-wide metadata can be viewed using the following command:

```
gcloud compute project-info describe --format="yaml(commonInstanceMetadata)"
```

All instance-specific metadata is available using the following command:

```
gcloud compute instances describe <INSTANCE>
```

# Persistent storage

To create the new persistent disk, run the following command:

```
gcloud compute disks create example-spd --size=10GB --zone=us-east1-c --type=pd-standard
```

Once created, we can attach the new disk to our hello-gce instance as follows:

```
gcloud compute instances attach-disk --zone=us-east1-c  gce-sample-app --disk=example-spd
```

With the disk successfully attached, we can validate that our VM detects the disk with the following command:

```
gcloud compute ssh --zone=us-east1-c gce-sample-app -- lsblk
```

# Managed instance group (MIG)

MIG is used for effective management of VM fleets.

1. Create a template:

```
gcloud compute instance-templates create migs-template-v1 \ 
       --machine-type f1-micro \
	   --region us-east1 \
	   --tags http-server \
	   --metadata-from-file startup-script=./startup-script-v1.sh
```

2. Let's create a small MIG with three instances by executing the following command:

```
gcloud compute instance-groups managed create simple-migs \
       --template migs-template-v1 \
	   --size 3 \
	   --region us-east1
```

Once executed, we can view our new MIG in the Cloud Console at Navigation menu | Compute Engine | Instance groups. Click on simple-migs to view information about the new group. You'll see that there are three instances running, each with a new external IP.

To tie all instances to a single IP address, Google offers Google Cloud Load Balancer (GCLB). These load balancers go far beyond simply distributing requests across instances, with smart features such as Anycast IPs, instance health checks, global presence, and integrations for auto-scaling. Managed instance groups integrate deeply with Google load balancers.


