# App engine additions

In addition to external service integrations, Google Cloud provides a few tools geared specifically toward running services on App Engine:

* task queues for long-running processes
* the App Engine Cron service for scheduling tasks within App Engine
* App Engine memcache for high-performance caching
* integrated email services.

# The hierarchical model of App Engine (see slides/app_engine.jpg):

* The service (fka the module) represents an individual web service such as an API or front-end web application
* Each App Engine service consists of one or more versions , which represent a unique implementation of the service
* App Engine instances represent a running process of the given service and version

# Simple App Engine Application

See the directory code/04 for code

To run this application locally, call the App Engine Python development server with: ```dev_appserver.py app.yaml``` 
The Python development server is included in the Google Cloud SDK app-engine-python component. If this is the first time running dev_appserver.py , then it's needed to install this component first. 
One can install it directly by running 

```
gcloud components install app-engine-python
```

# Deployment workflow

1. Source code is pushed to a temporary Cloud Storage bucket

2. Google Container Builder compiles the source code and packages the application into a new Docker image

The Google Container Builder is a managed service for building various application artifacts, primarily Docker images. When deploying a service to the flexible environment, Container Builder reads the app.yaml configuration file to determine how to build and package the source code into a Docker image. For custom runtimes, the Container Builder will create the image based on the provided Dockerfile.

3. The image is tagged with the service's version and stored in the Google Container Registry (GCR)

Successfully built images are stored in the Google Container Registry. Container Registry is a managed private Docker image registry. 
To view App Engine images in the Cloud Console, navigate to Navigation menu | Container Registry | Images. App Engine images are stored in the ```appengine``` folder. 
The image can be pulled to a local machine or the cloud shell.


4. A new managed Compute Engine VM is created based on the requested resources
5. The Docker image is deployed to the VM



 
