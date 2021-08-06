# GCS basics

Cloud Storage is a simple and easy to use general purpose object store, perfect for getting applications up and running with minimal complexity or operational overheads. However, a deeper look shows a full-fledged storage solution packed with features that can be tailored to a wide variety of specialized use cases. A handful of these features include the following:

* Globally consistent object lookups
* Automatic object versioning and retention
* Support for static web content and edge caching
* Metadata-driven decompressive transcoding
* At rest encryption and support for user-provided encryption keys
* Fully managed load balancing, multi-region availability, and scaling
* Automated data migration strategies from hot to cold storage classes
* Deep integration with many GCP and third-party products


# Creating a bucket

The protocol prefix gs:// is used to denote that a given path resides in Cloud Storage. From within the Cloud Shell, execute the following command to generate a unique bucket name:

```
export BUCKET_NAME=gs://hello-cloud-storage-$RANDOM
```

With a unique name generated, use the gsutil mb (make bucket) command to create a new bucket:

```
gsutil mb $BUCKET_NAME
```

Now that our bucket is created, you can view information about the bucket itself with the gsutil ls command by providing the optional -b flag to specify the bucket, and -L to display additional information:

```
gsutil ls -bL $BUCKET_NAME
```


# Uploading a file to the bucket

```
echo -e Hello, World > hello.txt
gsutil cp hello.txt $BUCKET_NAME
```

# Storage classes and locations

Cloud Storage offers four distinct storage classes:
(1) Regional Storage
(2) Multi-Regional Storage
(3) Nearline Storage
(4) Coldline Storage

All Cloud Storage classes provide a very similar experience in most aspects, including the following:

* A single set of APIs and tools
* Low latency using Time To First Byte (TTFB)
* 11 nines durability through erasure-coding
* A very rich shared feature set

One can check the storage using the following command:

```
gsutil stat $BUCKET_NAME/hello.txt
```

# Automating object management

Being able to migrate objects between hot and cold storage classes creates significant cost saving opportunities. Cloud Storage supports automated migration strategies through Object Lifecycle Management.

Object Lifecycle Management is configured on a per-bucket basis. Developers specify one or more conditions as well as an action to take. Supported actions may be either SetStorageClass or Delete . The action will be applied only when all co nditions are met. Supported conditions include the following:

* Age (number) : The number of days since the object was initially created.
* IsLive (boolean) : False when a versioned object is not the most recent version.
* CreatedBefore (date) : True for any object with a creation time before the specified date.
* MatchesStorageClass (string) : True for objects that match one of the provided storage classes.
* NumberOfNewerVersions (number) : For versioned objects, true when N or more newer versions of the object exist.

 

# Integrations

Some of the other GCP products that integrate with Cloud Storage include the following:

* Cloud Dataflow : Pipeline I/O for reading and writing Cloud Storage objects
* BigQuery : Import/export and query CSV, JSON, and AVRO files directly from Cloud Storage
* Cloud Dataproc : Cloud Storage as an incredibly scalable HDFS backend
* Cloud Functions : Object change triggers
* Cloud Load Balancers : Backend buckets for serving static content
* Cloud Pub/Sub :Publish notifications for object changes
* Cloud CDN :SSL and edge caching for static content in Cloud Storage
* Cloud Dataprep :For sanitizing and formatting Cloud Storage files for further use
* Cloud Logging : Export logs to Cloud Storage for long-term retention

# Concrete example: integrating with Google Cloud Functions

Before continuing, check that the Google Cloud Vision API is enabled for this project: navigate to https://console.developers.google.com/apis/api/vision.googleapis.com/overview and click the Enable Cloud Vision API button.

The codebase can be found in code/10a directory


 
