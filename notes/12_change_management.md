# Change management

To manage changes in codebase in an automatic way, there are such tools as:

* Cloud Source Repositories ( CSR )
* Cloud Deployment Manager
* Google Container Registry ( GCR )
* Container Builder

# Cloud Source Repositories

Creating Source Repositories requires the __source.repos.create__ permission, which is included in the Project Owner and Source Repositories Administrator IAM roles. 


A repository can be initialized in one of the following three ways:

* Push code from a local Git repository
* Clone the new repository locally
* Configure automatic mirroring from GitHub or Bitbucket

Once the repository is initialized, any future commits to your fork on GitHub will automatically propagate to your Google Cloud Source Repository. 
For GitHub, this is achieved through a traditional push event webhook to https://source.developers.google.com/webhook/github and a related deploy key. 
For Bitbucket, this is achieved through Bitbucket services as a post to https://source.developers.google.com/webhook/ with a URL encoded authentication key.

To test the mirroring functionality, modify the README.md file in the root of local repository and execute the following commands:

```
git add README.md
git commit -m "test commit"
git push origin master
```

After a moment, the changes will be reflected in the mirrored repository. This can be seen in the Cloud Console CSR commit history view.

NOTE: When creating a connected repository, Google creates it as a read-only mirror. 
This means developers cannot directly push changes to the source repository, and must instead push changes to the GitHub or Bitbucket repository

# Google Cloud Deployment Manager

Google Cloud Deployment Manager is a fully managed, declarative configuration management service built specifically for the Google Cloud ecosystem.

Cloud Deployment Manager operates on the basis of a defined desired state. This desired state is defined using configurations, which are YAML files that define a collection of GCP resources. Configurations may also be defined using JSON files, although this isn't recommended as YAML provides better support for some advanced features.

A basic configuration file takes the following format:
 
```
resources:
- name: <RESOURCE_NAME>
  type: <RESOURCE_TYPE>
  properties:
    <KEY>: <VALUE>    
	...
```

[Managed base types](https://cloud.google.com/deployment-manager/docs/configuration/supported-resource-types) take the form of <API>.<VERSION>.<RESOURCE>, such as compute.v1.instance for a Compute Engine instance.

Creating and managing deployment resources requires the following foles:
* Deployment Manager Editor role
* Project Editor role/Project Owner role.

The Deployment Manager API must be enabled for the given project. To do this, navigate to https://console.cloud.google.com/apis/library and search for the Google Cloud Deployment Manager V2 API. Select the API and click ENABLE.

Next, create a deployment containing a single Compute Engine instance to serve a static HTML file.

To create the deployment, first update the YAML by replacing all occurrences of YOUR_PROJECT_ID with your project ID. Next, create a deployment using the configuration file with the following command:

```
gcloud deployment-manager deployments create example-deployment --config simple-app-server.yaml 
```

Once the deployment has been created, the resulting cloud resources will be listed along with their type and current state. The deployment information can then be viewed from within the Cloud Console by navigating to Products & services | Deployment Manager | Deployments

The external IP  and manifests can be retrieved with the following commands:

```
gcloud compute instances describe app-server-1 --zone us-east1-b | grep natIP
gcloud deployment-manager manifests list --deployment example-deployment
```

# Google Container Registry 

GCR is a private Docker registry backed by Cloud Storage. GCR supports hosting images in Docker image manifest V2 and OCI formats.

# Container Builder

Container Builder is Google's fully managed build and workflow automation service.

To instruct Container Builder to build and publish a new Docker image, submit a new build with the desired tag and build context. Doing so requires the Cloud Container Builder IAM role or equivalent IAM permissions. From within the code/12a directory, execute the following command:

```
gcloud container builds submit  --tag us.gcr.io/PROJECT_ID/simple-app-server:0.0.2.
```

Container Builder will begin building the image, streaming all build logs and relevant information back to the caller. After building, Container Builder will automatically publish the image to the specified Container Registry host. Pull and run the image locally, binding to port 8080 on the host, as follows:

```
docker run --rm -it -p 8080:8080  us.gcr.io/PROJECT_ID/simple-app-server:0.0.2
```

