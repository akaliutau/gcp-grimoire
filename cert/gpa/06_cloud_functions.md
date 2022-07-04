# Environment

Cloud Functions run in a fully-managed serverless environment where Google handles infrastructure, operating systems, and runtime environments completely on your behalf. Each function runs in its own isolated secure execution context, scales automatically, and has a lifecycle independent from other functions.

Cloud Functions handles incoming requests by assigning them to instances of your function. Depending on the volume of requests, as well as the number of existing function instances, Cloud Functions may assign a request to an existing instance or create a new one.

Note: An extremely rapid increase in inbound traffic can intermittently cause some requests to fail with an HTTP code of 500. This is because the requests time out in the pending queue while waiting for new instances to be created.

# Execution guarantees

- HTTP functions are invoked at most once - due to synchronous nature of HTTP calls, and it means that any error that occurs during function invocation will be returned without retrying. The caller of an HTTP function is expected to handle errors and retry if needed.
- Event-driven functions are invoked at least once - because of the asynchronous nature of events, in which there is no caller that waits for the response.

# Best practices

The simplicity of Cloud Functions lets you quickly develop code and run it in a serverless environment. At moderate scale, the cost of running functions is low, and optimizing your code might not seem like a high priority. As your deployment scales up, however, optimizing your code becomes increasingly important.

- Guard against excessive scale-ups via flag `--max-instances`
- Request handling when all instances are busy



## Optimizing networking:

- Reduce CPU time spent in establishing new connections at each function call.
- Reduce the likelihood of running out of connection or DNS quotas.

## Retrying Event-Driven Functions

Setting "retry on failure" flag causes your function to be retried repeatedly until it either successfully executes or the maximum retry period has elapsed, which can be multiple days

# Limits

- max memory - 8GB
- max timeout - 540s


