# HPC at GCE

Tightly coupled high performance computing (HPC) workloads often use the Message Passing Interface (MPI) to communicate between processes and virtual machine (VM) instances.

## Benefits

- VMs ready for HPC out-of-the-box. There is no need to manually tune performance, manage VM reboots, or stay up to date with the latest Google Cloud updates for tightly coupled HPC workloads.
- Networking optimizations for tightly-coupled workloads. Optimizations that reduce latency for small messages are included, which benefits applications that are heavily dependent on point-to-point and collective communications.
- Compute optimizations for HPC workloads. Optimizations that reduce system jitter are included, which makes single-node high performance more predictable.
- Consistent, reproducible performance. VM image standardization gives you consistent, reproducible application-level performance.
- Improved application compatibility. Alignment with the node-level requirements of the Intel HPC platform specification enables a high degree of interoperability between systems.

## Implementation

- Use compact placement policy
- Use compute-optimized instances
- Disable Simultaneous Multithreading
- Adjust user limits
- Set up SSH host keys
- Choose an NFS file system or parallel file system (NFS-based solutions such as Filestore and NetApp Cloud Volumes, or POSIX-based parallel file systems like Lustre)
- Choose a storage infrastructure (SSD)
- Increase tcp_*mem settings (due to higher bandwidth)
- Use the network-latency profile
- Disable Linux firewalls
- Disable SELinux
- Turn off Meltdown and Spectre mitigation

Details:

- Placement policy (via `compute resource-policies create group-placement`) gives you control over the placement of your virtual machines (VMs) in data centers. Compact placement policy provides lower-latency topologies for VM placement in a single availability zone.

- Intel MPI requires host keys for all of the cluster nodes in the `~/.ssh/known_hosts` file of the node that executes mpirun

# Spot VMs (the latest version of preemptible VMs)

Compute Engine VMs use either the standard provisioning model (standard VMs) (default) or the spot provisioning model (Spot VMs). Unlike standard VMs, Spot VMs have significant discounts, but Compute Engine might preempt Spot VMs. Use Spot VMs to reduce costs for fault-tolerant workloads.

NOTE:
Managed instance groups can create or add new Spot VMs only when additional Compute Engine resources are available. If these resources are limited, managed instance groups are unable to resize or automatically scale the number of Spot VMs in the group.

# Custom images

One can create custom images using command `compute images create` from 
- source disks ( even while it is attached to a running VM)
- images
- snapshots of a persistent disk
- compressed images stored in Cloud Storage 

and use these images to create virtual machine (VM) instances. After creating a custom image, one can share it across projects. If you allow users from another project to use your custom images, then they can access these images by specifying the image project in their request. 

Custom images are ideal for situations where you have created and modified a persistent boot disk or specific image to a certain state and need to save that state for creating VMs.

Alternatively, one can use the virtual disk import tool to import boot disk images to Compute Engine from your existing systems and add them to your custom images list.

Note: If you want to create incremental backups of your persistent disk data, use snapshots instead.

## Best practice

To set up a life-cycle for images, use the deprecation command: `compute images deprecate` which allows to set the state (ACTIVE, DEPRECATED, OBSOLETE, DELETED) and successor 

Use image families to simplify image versioning. In case of roll back to a previous image version, deprecate the most recent image in the family.

# Instance templates

Definition: An instance template is an _immutable_ resource that can be used to create VM instances and MIGs, which includes the machine type, boot disk image or container image, labels, startup script, and other instance properties. Created via command `compute instance-templates create`  

An instance template is a global resource that is not bound to a zone or a region. However, you will specify some zonal resources in an instance template, which restricts the template to the zone where that resource resides. For example, if you include a read-only persistent disk from some zone in your instance template, you cannot use that template in any other zone because that specific disk exists only in this zone.
In that sense an instance template is a sort of recipe which is executed at specific location and can contain references only to resources at this location.

## Deterministic instance templates

In general, it is recommended that the properties of the instance template to be as explicit and deterministic as possible. F.e. provide explicit config information, such as the version of app to install via npm, etc
The reasons behind it _are exactly the same as behind `requirments.txt` file for python-based apps_.

# MIGs


- a MIG with VMs in a single zone (zonal MIG)	:  VMs to be deployed to a single zone
- a MIG with VMs in multiple zones in a region (regional MIG)	: to distribute your VMs across multiple zones in a region in order to protect against zonal failure or to automatically find zones with limited resource like Spot VMs
- a MIG with autoscaling	: to automatically create VMs in the group when demand increases and delete VMs when demand drops
- a MIG that uses preemptible VMs	: to take advantage of the cost-savings associated with preemptible VMs
- a MIG with stateful configuration	: if need a stateful configuration, f.e. disks that must retain their data whenever VMs are autohealed/updated/recreated

## Limitations

If you want to autoscale a regional MIG, the following limitations apply:

- must set the group's target distribution shape to EVEN.
- To scale in and out, must enable proactive instance redistribution.
 
A stateful MIG has the following limitations:

- cannot use autoscaling if your MIG has stateful configuration.
- for automated rolling updates, you must set the replacement method to RECREATE. For stateful regional MIGs, you must disable proactive redistribution (set the redistribution type to NONE) to prevent deletion of stateful instances by automatic cross-zone redistribution.

Stateful MIGs are intended for applications with stateful data or configuration, such as:

- Databases. For example: Cassandra, ElasticSearch, mongoDB, and ZooKeeper.
- Data processing applications. For example: Kafka and Flink. 
- Other stateful applications. For example: TeamCity/Jenkins/Bamboo (i.e. CI/CD tools), DNS servers with stateful IP address, and custom stateful workloads.
- Legacy monolith applications which rely on stateful configuration, such as specific VM instance names, IP addresses, or metadata key values.
- Batch workloads with checkpointing.

The difference between Stateful and Stateless apps is in stateful policy at former, which defines common stateful items for all instances in a managed instance group (to be specified in template).
A stateful managed instance group (stateful MIG) preserves the unique state of each virtual machine (VM) instance—including VM name, attached persistent disks, IP addresses, and/or metadata—on machine restart, recreation, auto-healing, or update.

# Sole-tenancy

VMs running on sole-tenant nodes can use the same Compute Engine features as other VMs, including transparent scheduling and block storage, but with an added layer of hardware isolation (each sole-tenant node maintains a one-to-one mapping to the physical server)

Use-cases for sole-tenant nodes:

- Gaming workloads with performance requirements.
- Finance or healthcare workloads with security and compliance requirements.
- Windows workloads with licensing requirements.
- Machine learning, data processing, or image rendering workloads. For these workloads, consider reserving GPUs.
- Workloads requiring increased input/output operations per second (IOPS) and decreased latency, or workloads that use temporary storage in the form of caches/processing space/low-value data. For these workloads, consider reserving local SSDs.


# Migration paths

## To import a large number of VM instances:

- use Migrate for Compute Engine tool
- best for importing multiple VM instances and their data.
- best for migrating VM instances from other cloud providers such as VMware and AWS.
- best for testing your apps in the cloud before you migrate. 

## To import a small number of VM instances (in OVA or OVF format):

- use Virtual Appliances (via `compute instances import` plus url for package)

A virtual appliance is a package that contains disk images and hardware configuration for a virtual machine (VM) instance (in the OVF/OVA format). An OVF package is a folder that contains an .ovf descriptor file and a collection of other resources, such as disks. When an OVF package is archived, it is referred to as an OVA file.

## To move VM between regions/projects:

use gcloud to create snapshots of VMs and re-create VMs at new project

## Limitations:

- cannot preserve the VM's ephemeral internal or external IP address, have to choose a new IP address when you recreate the VM.

# SSH connections

To manage user access to your Linux VM instances one can use the following methods:

- OS Login
- Managing SSH keys in metadata
- Temporarily grant a user access to an instance

NOTE:

Users who can connect to a VM can access the VM's metadata server to retrieve an access token. The access token lets users impersonate the VM's service account and use all IAM permissions granted to the service account, even if their account does not have the permissions required. You can mitigate this risk by using _OS Login_ which ensures that users who log in to a VM are granted permission to connect to the VM's (this can be done in usual way by binding necessary roles with User Account used on specific local machine)
OS Login simplifies SSH access management by linking your Linux user account to your Google identity. Administrators can easily manage access to instances at either an instance or project level by setting IAM permissions.


Compute Engine uses key-based SSH authentication to establish connections to all Linux virtual machine (VM) instances. You can optionally enable SSH for Windows VMs. By default, passwords aren't configured for local users on Linux VMs.

By default, Compute Engine uses custom project and/or instance metadata to configure SSH keys and to manage SSH access. When OS Login is enabled, one can connect faster using NSS service modules in your local Linux machine.
(When you connect to VMs using the gcloud CLI, Compute Engine creates a persistent SSH key for you). Note, that OS Login itself is configured via setting metadata flags as well. 

## Limitations

OS Login is not supported in the following products, features, and VMs:

- Cloud Data Fusion versions 6.1.4 and earlier
- Cloud Composer
- Google Kubernetes Engine (GKE) public clusters that run versions earlier than 1.23.5
- GKE private clusters that run node pool versions earlier than 1.20.5
- Dataproc Serverless
- Windows Server and SQL Server VMs
- Fedora CoreOS VMs. To manage instance access to VMs created using these images, use the Fedora CoreOS ignition system

# IP addresses 

To communicate with the internet, you can use an external IPv4 or external IPv6 address configured on the instance. If no external address is configured on the instance, Cloud NAT can be used for IPv4 traffic.

# Storage at VMs

Compute Engine offers several types of storage options for instances:

- Zonal persistent disk: Efficient, reliable block storage.
- Regional persistent disk: Regional block storage replicated in two zones.
- Local SSD: High performance, transient, local block storage.
- Cloud Storage buckets: Affordable object storage.
- Filestore: High performance file storage for Google Cloud users.

## Best practices

- Avoid temporary snapshots: use disk clones instead of snapshots if need a temp fast copy (reason - the snapshots are saved as delta sequence, and it takes more time to restore aa original disk)
- Schedule hourly snapshots for backup and disaster recovery
- Use images for fast and frequent disk creation across regions
- To create backups of all disks attached to a VM instance, use machine images

Use a machine image to store all the configuration, metadata, permissions, and data from multiple disks for a VM instance running on Compute Engine. The VM instance that you use to create a machine image is referred to as a source VM instance.

# Metadata

Every virtual machine (VM) instance stores its metadata on a metadata server. Your VM automatically has access to the metadata server API without any additional authorization. Metadata is stored as key:value pairs.

# Maintenance

You can choose how your virtual machines (VM) instances respond during or after a host event by setting the host maintenance policy. A host event can include the regular maintenance of Compute Engine infrastructure, or a host error on a VM. By default, VMs are set to live migrate during host system events, but you can set them to terminate and optionally restart.

A maintenance event is when Compute Engine stops a VM to perform a hardware or software update. If you enable the live migration host maintenance policy, Compute Engine moves the VM to a new host, and there is no disruption to your application.


# Secure Boot

Secure Boot helps ensure that the system only runs authentic software by verifying the digital signature of all boot components, and halting the boot process if signature verification fails.
Shielded VM instances run firmware which is signed and verified using Google's Certificate Authority, ensuring that the instance's firmware is unmodified and establishing the root of trust for Secure Boot.

Shielded VMs are instances with enhanced security controls, including the following:
- Secure boot
- vTPM
- Integrity monitoring

# Using labels

Labels are key-value pairs that can be used on Google Cloud to group related or associated resources. For example, on Compute Engine, you can use labels to group VMs in categories such as production, staging, or development and  take advantage of the nested filtering feature to perform more precise searches for your resources using labels.

Labels are a separate tool that allow you to create annotations for resources. 

Tags are structured as a key/value pair. A tag key resource can be created under your organization resource, and tag values are resources that are attached to a key, f.e. a tag key `environment` with values `PROD` and `DEV`.




