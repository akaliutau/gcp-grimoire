# Relational cloud databases

Google Cloud Platform offers two managed database services: Cloud SQL and Cloud Spanner
Cloud SQL exposes databases via traditional over-the-wire connection methods, 
and the developer experience is extremely similar to what one would get from a self-hosted MySQL or PostgreSQL database.

# Creating a Cloud SQL instance

In the Cloud Console navigate to Navigation menu | SQL and click CREATE INSTANCE. 
Cloud SQL supports both MySQL 5.6 and 5.7, and PostgreSQL 9.6.

Choose MySQL Second Generation in the creation dialog, and provide the following values:

* Instance ID : cloud-sql                                  <-- note this name for further commands
* Root password : Click Generate and save the password for later use
* Location.Region : us-central1
* Location.Zone : Any

Under the Show configuration options, set the following values:

* Machine type : db-f1-micro
* Storage type : HDD
* Enable auto backups : Uncheck Automate backups

 
The exact same instance could be created using the following gcloud command:

```
gcloud sql instances create cloud-sql \ 
       --no-backup \
	   --database-version=MYSQL_5_7 \
	   --region=us-central1 \
	   --storage-type=HDD \
	   --tier=db-f1-micro
```
 
# Connecting to Database

In order to permit access from external systems, Cloud SQL provides authorized networks , which specify whitelisted IP ranges in CIDR notation. For example, in order to authorize connections from all addresses, an authorized network could be added with an address range of 0.0.0.0/0

```
gcloud sql instances patch cloud-sql --authorized-networks=<CIDR_1>[, CIDR_2, ... ]
```

Note: here CIDR_1, ..., is the CIDR notation of the public IP of local machine

```
gcloud sql connect cloud-sql --user root
```

# Securing traffic


1. Download server's certificate

```
gcloud sql instances describe cloud-sql --format="value(serverCaCert.cert)" > server-ca.pem
```

2. Create a new client certificate on your behalf, run the following command (will save the private key to client.key):

```
gcloud sql ssl-certs create local-client-access client.key --instance=cloud-sql
```


3. To save the client certificate locally, run the following command:

```
gcloud sql ssl-certs describe local-client-access --instance cloud-sql --format="value(cert)" > client.pem
```

4. Turn on SSL:

```
gcloud sql instances patch cloud-sql --require-ssl
```

5. the following command can be used to connect using the standard MySQL client:

```
mysql --ssl-ca=./server-ca.pem \
      --ssl-cert=./client.pem \
	  --ssl-key=./client.key \
	  -h <INSTANCE_IP> -u root -p <ROOT_PASSWORD>
```

# Better approach: Cloud SQL Proxy

The Cloud SQL Proxy allows clients to establish a secure connection to Cloud SQL instances over an encrypted TCP tunnel, without the need for authorized networks, SSL certificates, or static IP addresses. 
The Cloud SQL Proxy runs a proxy client locally, with the proxy server running on the Cloud SQL instance. Clients simply interact with the proxy as if it were a MySQL or PostgreSQL database running on the same machine.
This approach has some resemblance with RPC technology


# Cloud Spanner

Spanner is the world's first globally consistent, horizontally scalable database with full ACID compliance.


# Performing a simple query

With the data in place, we can execute queries against our Spanner instance using familiar ANSI 2011 SQL. 
Navigate to your Cloud Spanner instance in the Cloud Console, select the library database, and click Query. 
One can retrieve the top 10 most prolific authors in our library with the following query:

```
SELECT Author.author_name, count(AuthorBook.isbn) as num_booksFROM AuthorJOIN AuthorBookON Author.author_id = AuthorBook.author_idGROUP BY Author.author_nameORDER BY num_books DESCLIMIT 10;
```

# Data collocation and interleaving

Cloud Spanner does not support arbitrary foreign key constraints, as doing so would introduce serious hurdles to how data is stored in a highly distributed environment. Instead, it supports the __interleaving__ of data, which provides both a method for implementing relational constraints, as well as a way to collocate related data for better performance.

When a new table is created in Cloud Spanner, it may declare another table to be its parent, forming a hierarchical structure between the tables. A child table must include the parent's primary ID as a component in its own primary key. If the parent's primary key is a composite key, the child table must include all fields of the composite key in its own primary key, with the same order.
