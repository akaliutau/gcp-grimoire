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


# Creating and using a bucket

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

 
