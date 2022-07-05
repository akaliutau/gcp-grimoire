
# FUSE

[Filesystem in Userspace (FUSE)](https://github.com/GoogleCloudPlatform/gcsfuse/) is a framework for exposing a filesystem to the Linux kernel. Note: Cloud Storage FUSE is _not_ a filesystem like NFS. It does not implement a filesystem or a hierarchical directory structure. It does interpret / characters in filenames as directory delimiters.

Cloud Storage FUSE is useful when you want to move files easily back and forth between Cloud Storage and a Compute Engine VM, a local server, or your development device using filesystem commands instead of `gsutil` commands or the cloud console.


# Requester Pays model

Whenever a user accesses a Cloud Storage resource such as a bucket or object, there are charges associated with making and executing the request. Such charges include:

- Data processing charges for operations, replication, and data retrieval.
- Network usage charges for reading the data.

Normally, the project owner of the resource is billed for these charges; however, if the requester provides a billing project with their request, the requester's project is billed instead. With Requester Pays enabled on your bucket, you can require requesters to include a billing project in their requests

The following restrictions apply when using Requester Pays:

- You cannot use a bucket that has Requester Pays enabled for imports and exports from Cloud SQL.

# Best practices for Cloud Storage

## Traffic

- Design your application to minimize spikes in traffic. If there are clients of your application doing updates, spread them out throughout the day.
- When designing applications for high request rates, be aware of rate limits for certain operations.


# Limits

- This limit is 63 characters if the name does not contain a dot (.) or 222 characters if the bucket contains a dot.
- There is a per-project rate limit to bucket creation and deletion. This rate limit is approximately 1 request every 2 seconds
- There is an update limit on each bucket. This limit is once per second, so rapid updates to a single bucket (for example, changing the CORS configuration) won't scale.
- There is a limit to the number of principals that can be granted IAM roles on a specific bucket. This limit is 1,500 principals for all IAM roles

Limits on objects:

- There is a maximum size limit for individual objects stored in Cloud Storage. This limit is 5 TiB
- The Cloud Storage access control system includes the ability to specify that buckets are publicly writable. If you need to make content available securely to users who don't have Google accounts we recommend you use signed URLs.


# Access control best practices

- Use the principle of least privilege when granting access to your buckets or objects
- Avoid granting IAM roles with setIamPolicy permission or granting the ACL OWNER permission to people you do not know.
- Be careful how you grant permissions for anonymous users.
- Be sure you delegate administrative control of your buckets (to prevent resources from becoming inaccessible, grant the Storage Admin IAM role for your project to a group instead of an individual)
