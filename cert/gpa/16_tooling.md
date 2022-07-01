# CLI tooling short reference

# Common operations

## kubectl

This is the syntax to run kubectl commands from console:

```
kubectl [command] [TYPE] [NAME] [flags]
```

where command, TYPE are:

command: Specifies the operation that you want to perform on one or more resources, for example `create`, `get`, `describe`, `delete`.

TYPE: Specifies the resource type, normally it's a noun in a plural form (but this is only in general)

## Examples

kubectl apply - Apply or Update a resource from a file or stdin.

Create a service using the definition in example-service.yaml.

```
kubectl apply -f example-service.yaml
```

Create a replication controller using the definition in example-controller.yaml.

```
kubectl apply -f example-controller.yaml
```

Create the objects that are defined in any .yaml, .yml, or .json file within the <directory> directory.

```
kubectl apply -f <directory>
```


kubectl get - List one or more resources.

List all pods in plain-text output format.

```
kubectl get pods
```

List all pods in plain-text output format and include additional information (such as node name).

```
kubectl get pods -o wide
```

## gsutil

some commands have a linux bash equivalent, such as `ls`, `cat`, etc 

- acl - operations with default object ACLs
- bucketpolicyonly -  is used to retrieve or configure the uniform bucket-level access setting of Cloud Storage buckets 
- cat
- compose - creates a new object whose content is the concatenation of a given sequence of source objects under the same bucket
- config - the gsutil config command obtains access credentials for Cloud Storage and writes a boto/gsutil configuration file containing the obtained credentials along with a number of other configuration-controllable values.
- cors - gets or sets the Cross-Origin Resource Sharing (CORS) configuration on one or more buckets.
- cp
- defacl - manipulations with default object ACLs for the specified buckets
- defstorageclass - operations with default storage class for the specified bucket(s)
- du - displays the amount of space in bytes used up by the objects in a bucket, subdirectory, or project.
- hash
- help
- hmac
- iam - is used to get, set, or change bucket and/or object IAM permissions
- kms - is used to configure Cloud Storage and Cloud KMS resources to support encryption of Cloud Storage objects with Cloud KMS keys
- label - to do label configuration (also called the tagging configuration by other storage providers) of one or more buckets
- lifecycle -  to get or set lifecycle management policies for a given bucket.
- logging -  Usage/Storage logs provide information for all of the requests made on a specified bucket and are created hourly/daily.
- ls
- mb
- mv
- notification -  to configure Pub/Sub notifications for Cloud Storage and Object change notification channels (OBJECT_FINALIZE - An object has been created, OBJECT_METADATA_UPDATE - The metadata of an object has changed, OBJECT_DELETE - An object has been permanently deleted, OBJECT_ARCHIVE - A live version of an object has become a noncurrent version.)
- pap -  is used to retrieve or configure the public access prevention setting of Cloud Storage buckets.
- perfdiag
- rb
- requesterpays
- retention - configures a data retention policy for a Cloud Storage bucket that governs how long objects in the bucket must be retained. 
- rewrite -  rewrites cloud objects, applying the specified transformations to them (add, rotate, or remove encryption keys on objects,  to specify a new storage class for objects.)
- rm
- rpo
- rsync - makes the contents under dst_url the same as the contents under src_url, by copying any missing files/objects (or those whose data has changed), and (if the -d option is specified) deleting any extra files/objects. 
- setmeta
- signurl - generates a signed URL that embeds authentication data so the URL can be used by someone who does not have a Google account
- stat
- test
- ubla
- update
- version
- versioning
- web - allows to configure a bucket to behave like a static website when the bucket is accessed through a custom domain.

## Examples

```
gsutil signurl -d 10m <private-key-file> gs://<bucket>/<object>
gsutil -i <service account email> signurl -d 10m -u gs://<bucket>/<object>
gsutil web set [-m <main_page_suffix>] [-e <error_page>] gs://<bucket_name>
```

If an object is shared publicly, you can also view that object with the URL:  http://storage.googleapis.com/BUCKET_NAME/OBJECT_NAME

For example, the URL for an index.html object would be:  http://storage.googleapis.com/www.example.com/index.html

## gcloud


General commands

```
gcloud init: Initialize, authorize, and configure the gcloud CLI.
gcloud version: Display version and installed components.
gcloud components install: Install specific components.
gcloud components update: Update your gcloud CLI to the latest version.
gcloud config set project: Set a default Google Cloud project to work on.
gcloud info: Display current gcloud CLI environment details.
```

Configuration

```
gcloud config set: Define a property (like compute/zone) for the current configuration.
gcloud config get-value: Fetch the value of a gcloud CLI property.
gcloud config list: Display all the properties for the current configuration.
gcloud config configurations create: Create a new named configuration.
gcloud config configurations list: Display a list of all available configurations.
gcloud config configurations activate: Switch to an existing named configuration.
```

Credentials: grant and revoke authorization to the gcloud CLI.

```
gcloud auth login: Authorize Google Cloud access for the gcloud CLI with Google Cloud user credentials and set the current account as active.
gcloud auth activate-service-account: Like gcloud auth login but with service account credentials.
gcloud auth list: List all credentialed accounts.
gcloud auth print-access-token: Display the current account's access token.
gcloud auth revoke: Remove access credentials for an account.
```

Projects
Manage project access policies.

```
gcloud projects describe: Display metadata for a project (including its ID).
gcloud projects add-iam-policy-binding: Add an IAM policy binding to a specified project.
```

IAM
Configuring Identity and Access Management (IAM) preferences and service accounts.

```
gcloud iam list-grantable-roles: List IAM grantable roles for a resource.
gcloud iam roles create: Create a custom role for a project or org.
gcloud iam service-accounts create: Create a service account for a project.
gcloud iam service-accounts add-iam-policy-binding: Add an IAM policy binding to a service account.
gcloud iam service-accounts set-iam-policy-binding: Replace existing IAM policy binding.
gcloud iam service-accounts keys list: List a service account's keys.
```

Docker & Google Kubernetes Engine (GKE)
Manage containerized applications on Kubernetes.

```
gcloud auth configure-docker: Register the gcloud CLI as a Docker credential helper.
gcloud container clusters create: Create a cluster to run GKE containers.
gcloud container clusters list: List clusters for running GKE containers.
gcloud container clusters get-credentials: Update kubeconfig to get kubectl to use a GKE cluster.
gcloud container clusters resize: change the size of cluster.
gcloud container images list-tags: List tag and digest metadata for a container image.
```

Virtual Machines & Compute Engine
Create, run, and manage VMs on Google Cloud infrastructure.

```
gcloud compute zones list: List Compute Engine zones.
gcloud compute instances create: Create a VM instance.
gcloud compute instances describe: Display a VM instance's details.
gcloud compute instances list: List all VM instances in a project.
gcloud compute instance-groups managed resize: resize the MIG
gcloud compute instances simulate-maintenance-event
gcloud compute disks snapshot: Create snapshot of persistent disks.
gcloud compute snapshots describe: Display a snapshot's details.
gcloud compute snapshots delete: Delete a snapshot.
gcloud compute ssh: Connect to a VM instance by using SSH.
gcloud compute sole-tenancy node-templates create: the same as templates but for sole-tenancy use-case
gcloud compute sole-tenancy node-groups create:    the same as templates but for sole-tenancy use-case

```

Serverless & App Engine
Build highly scalable applications on a fully managed serverless platform

```
gcloud app deploy: Deploy your app's code and configuration to the App Engine server.
gcloud app versions list: List all versions of all services deployed to the App Engine server.
gcloud app browse: Open the current app in a web browser.
gcloud app create: Create an App Engine app within your current project.
gcloud app logs read: Display the latest App Engine app logs.
```

Miscellaneous
Commands that might be useful

```
gcloud kms decrypt: Decrypt ciphertext (to a plaintext file) using a Cloud Key Management Service key.
gcloud logging logs list: List your project's logs.
gcloud sql backups describe: Display info about a Cloud SQL instance backup.
gcloud sql export sql: Export data from a Cloud SQL instance to a SQL file.
```

Flags: Set after positional args; order of flags doesn’t matter.

A flag can be either a:

- Name-value pair (--foo=bar)
- Boolean (--force/no-force)

Additionally, flags can either be:

- Required
- Optional: If an optional flag is not defined, the default value is used

Global flags

Some flags are available throughout the gcloud CLI experience, like:

```
--help: For when in doubt; display detailed help for a command.
--project: If using a project other than the current one.
--quiet: Disabling interactive prompting (and applying default values for inputs).
--verbosity: Can set verbosity levels at debug, info, warning, error, critical, and none.
--version: Display gcloud version information.
--format: Set output format as config, csv, default, diff, disable, flattened, get, json, list, multi, none, object, table, text, value, or yaml.
```

## cbt

Usage:

```
cbt [-<option> <option-argument>] <command> <required-argument> [optional-argument]
```

The commands are:

```
count                     Count rows in a table
createinstance            Create an instance with an initial cluster
createcluster             Create a cluster in the configured instance
createfamily              Create a column family
createtable               Create a table
updatecluster             Update a cluster in the configured instance
deleteinstance            Delete an instance
deletecluster             Delete a cluster from the configured instance
deletecolumn              Delete all cells in a column
deletefamily              Delete a column family
deleterow                 Delete a row
deleteallrows             Delete all rows
deletetable               Delete a table
doc                       Print godoc-suitable documentation for cbt
help                      Print help text
import                    Batch write many rows based on the input file
listinstances             List instances in a project
listclusters              List clusters in an instance
lookup                    Read from a single row
ls                        List tables and column families
mddoc                     Print documentation for cbt in Markdown format
read                      Read rows
set                       Set value of a cell (write)
setgcpolicy               Set the garbage-collection policy (age, versions) for a column family
waitforreplication        Block until all the completed writes have been replicated to all the clusters
version                   Print the current cbt version
createappprofile          Create app profile for an instance
getappprofile             Read app profile for an instance
listappprofile            Lists app profile for an instance
updateappprofile          Update app profile for an instance
deleteappprofile          Delete app profile for an instance
```

The options are:

```
-project string    project ID. If unset uses gcloud configured project
-instance string   Cloud Bigtable instance
-creds string      Path to the credentials file. If set, uses the application credentials in this file
```