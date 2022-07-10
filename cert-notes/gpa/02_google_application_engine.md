
# Differences: The App Engine flexible environment <-> Compute Engine:

- The flexible environment VM instances are restarted on a weekly basis (to apply any necessary operating system and security updates)

- You always have root access to Compute Engine VM instances. By default, SSH access to the VM instances in the flexible environment is disabled.

- Code deployments can take longer as container images are built by using the Cloud Build service.

# Standard Environment

- There are six supported languages: Python 3, Java 11, Node.js, PHP 7, Ruby, and Go
- There is a mem limit in 2GB for instances (B8 class)

# Flexible Environment

- Customizable infrastructure - App Engine flexible environment instances are Compute Engine virtual machines, which means that you can take advantage of custom libraries, use SSH for debugging, and deploy your own Docker containers.
- Performance options - Take advantage of a wide array of CPU and memory configurations. You can specify how much CPU and memory each instance of your application needs.
- Native feature support - Features such as microservices, authorization, SQL and NoSQL databases, traffic splitting, logging, versioning, security scanning, and content delivery networks are natively supported.
- Managed virtual machines - App Engine manages your virtual machines


# Memcache service at GAE

- Shared memcache is the free default for App Engine applications. It provides cache capacity on a best-effort basis and is subject to the overall demand of all the App Engine applications using the shared memcache service.

- Dedicated memcache provides a fixed cache capacity assigned exclusively to your application. It's billed by the GB-hour of cache size and requires billing to be enabled. Having control over cache size means your app can perform more predictably and with fewer reads from more costly durable storage.

Limits: 100GB for the US, 20GB for other regions