**ServiceLoader** is a Java mechanism that allows to perform DI in plain java. To use it, application must provide an interface or abstract class that acts as a proxy to injected service. It’s named a *Service Provider Interface* (SPI). The ServiceProvider is allowed to contain one or more concrete classes that implement SPI. It is achieved with special configuration.

A ServiceProvider is identified by placing a configuration file in the resource directory `META-INF/services`. The file’s name is the fully-qualified binary name of the service’s type. The file contains a list of fully-qualified binary names of concrete provider classes, one per line. In the application these implementations can be loaded with the following code, which returns an iterable of all loaded services:

- `ServiceLoader<MyService> serviceLoader = ServiceLoader.load(MyService.class);`

Also, this configuration can be added programmatically with `@AutoService` annotation of `com.google.auto.service:auto-service` library.

[source 1](https://pedrorijo.com/blog/java-service-loader/); [source 2](https://www.baeldung.com/java-spi#2-service-provider-interface); [source 3](https://habr.com/ru/company/otus/blog/457440/);
