A service typically calls other services and databases as well. For each environment like dev, QA, UAT, prod, the endpoint URL or some configuration properties might be different. A change in any of those properties might require a rebuild and redeploy of the service.

To avoid code modification configuration can be used. Externalize all the configuration, including endpoint URLs and credentials. Applications should load them either at startup or on the flight. These can be accessed on the application startup or refreshed without a server restart.
