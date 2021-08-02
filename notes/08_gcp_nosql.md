# Datastore as a NoSql solution at GCP

Datastore, like all NoSQL solutions, makes compromises in key areas in order to achieve things that traditional relational databases cannot. 

Datastore only supports a basic level of query functionality, including equality and partial inequality, with several limitations. 
Datastore does not support traditional join or aggregation operations. 

Datastore may be configured to run in either regional or multi-regional locations. A regional location allows for lower write latency than multi-regional locations. Because there are more options on where to run regional configurations, this option also allows for better collocation of data with other Google Cloud services, or with a geographically concentrated user base.

# Terminology

|   Relational databases |       Datastore            |
|------------------------|----------------------------|
|     Table              |         Kind               |
|      Row               |        Entity              |
|  Column or field       |       Property             |
|    Primary key         |    Identifier, key         |
|    Foreign keys        |  Ancestors and descendants |


Each entity constitutes a discrete object in the database, and every entity is of a specific kind (==category) of entities. 
Every entity in Datastore is assigned a unique identifier when created, similar to primary keys of a relational database. 
Identifiers are unique for the entity's namespace and kind, and may take one of two forms: a string name or a numerical ID

In addition to kind, each entity is composed of one or more properties of various types, including numbers such as integers and doubles, plus strings, timestamps, Booleans, and blobs. 
Additionally, there are a few more complex types such as arrays, embedded entities, and geographic points.


Whereas relational databases rely on well-defined schemata to guarantee uniformity of data within a table, 
Datastore does not impose any uniformity on the properties of a given entity. This means that two entities of the same kind could have entirely different sets of properties.
Note: there is no guarantee that a given field will be available for any single entity. 
As a result, any structural uniformity between entities must be implemented at the application level.


# Data ingestion

Install required dependencies and execute the script:

```
pip install -r requirements.txt
python generate-data.py
```

# Queries with GQL

Example:

```
SELECT * FROM Employee WHERE name = 'Eva Evans'
```

Datastore creates indexes for all entities based on their ancestry path, kind, and name or ID. By default, all properties of an entity are also indexed, unless explicitly excluded from indexing.

To create the custom index, either of the following commands can be executed:

```
gcloud app create index.yaml # (using the App Engine API)
gcloud datastore create-indexes index.yaml # (using the Datastore API)
```

# Permissions in Datastore

All App Engine applications have full access to Datastore APIs, and full administrative authority in Datastore requires the App Engine App Admin role. 
In addition to the primitive Google Cloud roles (owner, editor, viewer), Datastore supports six specific IAM roles:

* roles/datastore.owner : Allows full access to Datastore resources with the exception of admin functions
* roles/appengine.appAdmin : Provides admin functions to roles/datastore.owner
* roles/datastore.user : Application-data level operations such as entity read/writes
* roles/datastore.viewer : Read-only access to Datastore entities
* roles/datastore.importExportAdmin : Datastore import and export operations
* roles/datastore.indexAdmin : Datastore index management operations

# Bigtable

Bigtable is an ideal storage solution for extremely large datasets containing billions of rows, each containing potentially thousands of columns. It thrives in situations where both reads and writes need to be extremely fast and cheap, such as map/reduce and time-series analysis.

Bigtable is generally considered a member of the wide-column store family of NoSQL databases,

Every row in Bigtable is indexed by a single row key - arbitrary byte strings that may be up to 64 KB (should generally be limited to under 1 KB for performance and cost). Bigtable persists rows in a sorted order by these row keys, making it possible to retrieve several related rows quickly as a single, contiguous scan over the row key.

# Creating a development cluster

(Note the cost - delete this instance when all tests are finished)

In Cloud Console navigate to Navigation menu | Bigtable and click Create instance. 

Instances require a name, ID, cluster ID, zone, and storage type. For our purposes, provide the following values to minimize cost:

* Instance name : sample-bigtable
* Instance ID : sample-bigtable
* Instance type : development
* Cluster ID : sample-bigtable-cluster
* Zone : us-east1-b
* Storage type : HDD

For example:

```
gcloud beta bigtable instances create sample-bigtable \
       --display-name=sample-bigtable \
	   --instance-type=DEVELOPMENT \
	   --cluster=sample-bigtable-cluster \
	   --cluster-zone=us-east1-b \
	   --cluster-storage-type=hdd
```

# Querying

1. Install cli for BigTable:

```
gcloud components install cbt
```
 
2. Create table, populate it with data:

```
cbt -instance sample-bigtable createtable employees
cbt -instance sample-bigtable ls

cbt -instance sample-bigtable createfamily employees details

cbt -instance sample-bigtable set employees \
    ceo:alisa_atwood \
	details:name="Alisa Atwood" \
	details:title="CEO" \
	details:description="Alisa is a renowned leader in the community."
```

# BigQuery

BigQuery is a fully managed analytics data warehousing solution on Google Cloud. One feature of BigQuery is the ability to query data from external sources, including Bigtable.


