In a MSA, requests often span multiple services. Each service handles a request by performing one or more operations across multiple services. While in troubleshooting it is worth having a trace ID, we should trace requests end-to-end. The solution is to introduce a transaction ID (Correlation ID). Following approach can be used:
- Assign each external request a unique request id.
- Pass the external request id to all services.
- Include the external request id in all log messages.
