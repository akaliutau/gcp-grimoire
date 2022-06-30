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
- Other stateful applications. For example: TeamCity, Jenkins, Bamboo, DNS servers with stateful IP address, and custom stateful workloads.
- Legacy monolith applications which rely on stateful configuration, such as specific VM instance names, IP addresses, or metadata key values.
- Batch workloads with checkpointing.

The difference between Stateful and Stateless apps is in stateful policy at former, which defines common stateful items for all instances in a managed instance group (to be specified in template).
A stateful managed instance group (stateful MIG) preserves the unique state of each virtual machine (VM) instance—including VM name, attached persistent disks, IP addresses, and/or metadata—on machine restart, recreation, auto-healing, or update.





