
# Best practices for importing and exporting

- Use the same SQL mode for import and export: if you export from a database without Strict SQL enabled, then try to import to Cloud SQL (which enables Strict SQL by default), the import might fail.
- Don't use Cloud Storage Requester Pays buckets (limit from GCS)
- Use serverless export to offload the export operation from the primary instance: to prevent slow responses during an export, you can use serverless export. With serverless export, Cloud SQL creates a separate, temporary instance to offload the export operation (using `offload` flag)
- Use the correct flags when you create a SQL dump file.
- Compress data to reduce cost: cloud SQL supports importing and exporting both compressed and uncompressed files.
- Reduce long-running import and export processes (because of (1) cannot stop a long-running Cloud SQL instance operation and (2) one can perform only one import or export operation at a time for each instance)
- Use InnoDB (InnoDB is the only supported storage engine for MySQL instances)
- MySQL import and migration jobs containing metadata with DEFINER clause (because a MySQL import or migration job doesn't migrate user data, sources and dump files which contain metadata defined by users with the DEFINER clause will fail to be imported or migrated as the users do not yet exist there)
- Verify the imported database (f.e. connect and list the databases, tables, and specific entries, etc)

# References

## InnoDB MySQL

InnoDB is a general-purpose storage engine that balances high reliability and high performance.

Here are a few of the major differences between InnoDB and MyISAM:

- InnoDB has row-level locking. MyISAM only has full table-level locking.
- InnoDB has what is called referential integrity which involves supporting foreign keys (RDBMS) and relationship constraints, MyISAM does not (DMBS).
- InnoDB supports transactions, which means you can commit and roll back. MyISAM does not.
- InnoDB is more reliable as it uses transactional logs for auto recovery. MyISAM does not.


