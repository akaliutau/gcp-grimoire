
# BigQuery data loading

There are several ways to ingest data into BigQuery:

- Batch load a set of data records.
- Stream individual records or batches of records.
- Use queries to generate new data and append or overwrite the results to a table.
- Use a third-party application or service.

Options for batch loading in BigQuery include the following:

- Load jobs. Load data from Cloud Storage or from a local file by creating a load job. The records can be in Avro, CSV, JSON, ORC, or Parquet format.
- BigQuery Data Transfer Service. Use BigQuery Data Transfer Service to automate loading data from Google Software as a Service (SaaS) apps or from third-party applications and services.
- BigQuery Storage Write API. The Storage Write API lets you batch-process an arbitrarily large number of records and commit them in a single atomic operation. If the commit operation fails, you can safely retry the operation. Unlike BigQuery load jobs, the Storage Write API does not require staging the data to intermediate storage such as Cloud Storage.
- Other managed services. Use other managed services to export data from an external data store and import it into BigQuery. For example, you can load data from Firestore exports.

Options for Streaming in BigQuery include the following:

- Storage Write API. The Storage Write API supports high-throughput streaming ingestion with exactly-once delivery semantics.
- Dataflow. Use Dataflow with the Apache Beam SDK to set up a streaming pipeline that writes to BigQuery.
- BigQuery Connector for SAP. The BigQuery Connector for SAP enables near real time replication of SAP data directly into BigQuery.
