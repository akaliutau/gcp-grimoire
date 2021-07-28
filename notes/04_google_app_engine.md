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

