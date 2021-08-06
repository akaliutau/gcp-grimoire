# Definitions

## Pipelines

The primary means for interacting with Cloud Dataflow is through pipelines. 
A pipeline programmatically defines a data processing task from start to finish, taking data from 1+ sources, performing a series of transformations on that data, and exporting the results to 1+ destinations(sinks). Pipelines may follow a sequential path from one transformation to the next, or they may be complex with one-to-many, many-to-one, and many-to-many connections between transformations, forming a directed acyclic graph

## Collections

Dataflow pipelines operate on data in terms of collections, through the use of the abstract PCollection. Each PCollection:

* represents a distributed set of homogeneous data as it flows through the pipeline
* may represent a bounded data source, such as a specific CSV file in Cloud Storage, or an unbounded data source, such as a Cloud Pub/Sub topic.
* is immutable
* has an associated timestamp

## Transformations

Transformations are the basic building block of Cloud Dataflow pipelines, with each transformation representing a step of the overall processing task. Developers define each transformation by implementing PTransform , which accepts 1+ PCollection , operates on the elements within that collection, and returns 0+ PCollection as a result of those operations.
 
Each PTransform can be categorized into one of three types: element-wise transforms, aggregate transforms, or composite transforms.


