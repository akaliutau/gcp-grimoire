# Implementing SLOs

Service level objectives (SLOs) specify a target level for the reliability of your service. Because SLOs are key to making data-driven decisions about reliability, they’re at the core of SRE practices.
Also because they tend to be the basis to make decisions on the basis of some metrics, they should be very specific.

# Examples of SLO of different types

An SLO sets a target level of reliability for the service’s customers. 

- Above this threshold, almost all users should be happy with your service (assuming they are otherwise happy with the utility of the service)
- Below this threshold, users are likely to start complaining or to stop using the service.


## Availability:

Objective value = 97%
SLI is the name of metric is used to measure availability, f.e. ratio of time servers are up

## Latency

90% of requests < 450 ms
SLI is the name of metric is used to measure availability, f.e. the number of successful HTTP requests / total HTTP requests (success rate) within 450ms