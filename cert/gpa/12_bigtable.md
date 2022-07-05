# Use-cases

- Time-series data, such as CPU and memory usage over time for multiple servers.
- Marketing data, such as purchase histories and customer preferences.
- Financial data, such as transaction histories, stock prices, and currency exchange rates.
- Internet of Things data, such as usage reports from energy meters and home appliances.
- Graph data, such as information about how users are connected to one another.

# Application profiles

If you created the instance with 1 cluster, the default app profile uses single-cluster routing, and it enables single-row transactions. This ensures that adding additional clusters later doesn't change the behavior of your existing applications.
If you created the instance with 2 or more clusters, the default app profile uses multi-cluster routing. Single-row transactions are never allowed with multi-cluster routing.