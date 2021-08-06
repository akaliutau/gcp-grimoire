# Site Reliability Engineering (SRE)


Site reliability engineering (SRE) is a set of principles and practices[1] that incorporates aspects of software engineering and applies them to infrastructure and operations problems.[2] The main goals are to create scalable and highly reliable software systems.[2] Site reliability engineering is closely related to DevOps, a set of practices that combine software development and IT operations, and SRE has also been described as a specific implementation of DevOps

https://en.wikipedia.org/wiki/Site_reliability_engineering

# Monitoring and alerting

Google SRE prioritizes monitoring four key aspects of a user facing system, the four golden signals:

* Latency : The time it takes to service a user's request, ideally measured separately for successful responses and error responses
* Traffic : The overall volume of requests a system is experiencing
* Errors : The total rate of requests that fail, defined in clear terms that fit the given system
* Saturation : The overall load on a system compared to its total capacity or target utilization


# Stackdriver
 
Stackdriver began as an independent monitoring product.

Some of the key Stackdriver offerings include:

* Monitoring applications, infrastructure, and managed services
* Centralized logging with search and reporting
* User-defined alerting policies for logs and metrics
* Live debugging to diagnose issues on running systems
* Network tracing to identify sources of latency
* Application profiling to maximise performance and reduce waste

# Creating a Stackdriver account

Every Stackdriver account belongs to exactly one GCP project. The Stackdriver account can be created as part of an existing project or as part of a dedicated Stackdriver project.

This can be done from within the Cloud Console under Navigation menu | Monitoring, which will navigate away from the Cloud Console to the Stackdriver Monitoring interface (https://apps.google.stackdriver.com).


# Stackdriver Logging

All logs in Stackdriver Logging share a uniform structure, known as the log entry . This structure includes both the logged information and several fields related to the log event, notably the following:

* Log name: The fully qualified GCP resource name from which the entry originated.
* Resource: An object representations of the originating GCP resource, including resource type and any labels associated with that resource.
* Log event information: Attributes associated with the conditions under which the log entry occured. This includes severity, log creation time, received time, severity, and Cloud Trace information.
* Labels: Default and user-provided labels attached to the log entry.
* HTTP Request: An object representation of the HTTP request associated with the log, if any.
* Payload: The actual information logged. This can take the form of one of the following: protoPayload (a protocol buffer), textPayload, or jsonPayload.



 