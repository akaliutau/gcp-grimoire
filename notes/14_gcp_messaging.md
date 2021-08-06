# Definitions

Cloud Pub/Sub follows a publisher-subscriber messaging pattern, where publishers do not maintain any reference to recipients.

## Topic
When submitting a message to Cloud Pub/Sub, the message is written to a specific topic. Any number of subscriptions may be attached to a given topic, forming a one-to-many relationship. 

## Queue
For each subscription, Cloud Pub/Sub maintains a separate message queue. Each message posted to a given topic will be written to every subscription attached to that topic. 


