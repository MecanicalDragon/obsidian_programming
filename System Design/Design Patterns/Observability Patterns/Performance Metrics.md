When the service portfolio increases due to a microservice architecture, it becomes critical to keep a watch on the transactions so that patterns can be monitored and alerts sent when an issue happens.

A metrics service is required to gather statistics about individual operations. It should aggregate the metrics of an application service, which provides reporting and alerting. There’re 2 models for metrics aggregation:
- Push — the service pushes metrics to the metrics service e.g., New Relic, AppDynamics
- Pull — the metrics services pull metrics from the service e.g., Prometheus
