**Image** - a packed set of software that is ready to run.

**Container** - a running instance of an *image*.

**Image stream** provides a way of storing different versions of the same image. Those different versions are represented by different *tags* on the same *image* name.

**Image repository** is a collection of related container *images* and *tags* identifying them. By watching an *image stream*, `OpenShift Container Platform (OSCP)` can receive notifications when new images are added or modified and react by performing a *build* or *deployment*.

**Image registry** is a content server that can store and serve *images*. Registry contains a collection of one or more *image repositories*, which contain one or more tagged *images*.

**Node** – virtual or bare-metal machine in a Kubernetes cluster. Worker nodes host application containers, grouped in *pods*.

**Pod** is one or more containers deployed together on one host, and the smallest compute unit that can be defined, deployed, and managed. Each pod is allocated with its own internal IP address, therefore owning its entire port space, and containers within pods can share their local storage and networking.

**ConfigMap** provides mechanisms to inject containers with configuration data while keeping containers agnostic of OSCP. ConfigMap must be created before its contents can be consumed in pods. ConfigMaps reside in a project and can only be referenced by pods in the same project.

**Secret** provides a mechanism to hold sensitive information such as passwords, OSCP client configuration files, private source repository credentials, and so on. Secrets decouple sensitive content from the pods.

**Volume** is directory, accessible to the *Containers* in a *Pod*, where data is stored for the life of the pod. Volumes are mounted file systems available to pods and their containers which may be backed by a number of host-local or network attached storage endpoints.

**Build** is the process of transforming input parameters into a resulting object. Most often, the process is used to transform source code into a runnable image. *Build* is defined in a *BuildConfig* with a set of triggers for when a new build should be started. Resolved *Build* publishes an *Image* of an application to *ImageStream*.

**ReplicaSet** ensures that a specified number of pod replicas are running and up at any time. If any pod is down, *ReplicaSet* kills it and creates another one instead. It is often used to guarantee the availability of a specified number of identical *pods*.

**Deployment** is a high-level wrapper over the *ReplicaSet* based on a user defined template called *DeploymentConfig*. It just creates new replication controller (replicaSet) and lets it start up pods. But also it can track image versions, trigger *builds* and rollouts. *DeploymentConfig* object defines the following details of a deployment:
- The elements of a *ReplicationController* definition.
- *Triggers* for creating a new deployment automatically.
- The strategy for transitioning between deployments.
- Life cycle hooks.

**Service** serves as internal load balancer. It identifies a set of replicated pods in order to proxy the connections it receives to them. Backing *pods* can be added to or removed from a service arbitrarily while the service remains consistently available, enabling anything that depends on the service to refer to it at a consistent internal address. Services are assigned IP address and port pairs that, when accessed, proxy to an appropriate backing *pod*. A service uses a *label selector* to find all appropriate running containers. *Service can be accessible only within a cluster*. For external access *Routes* and *Ingresses* are used – they take pod endpoints from Service and route requests directly to them. In Kubernetes, there are several types of services available to manage network connectivity to pods. Here are the commonly used types of services:
- **ClusterIP**: This is the default service type. It exposes the service on a cluster-internal IP address, accessible only within the cluster. It allows communication between services and pods within the cluster, but it is not accessible from outside the cluster.
- **NodePort**: This type of service exposes the service on a static port on each *node* in the cluster. It creates a listener on the specified port on every node and forwards traffic to the service. *NodePort services are accessible from outside the cluster* by using any node's IP address and the assigned static port.
- **LoadBalancer**: This service type creates an external load balancer in the cloud providers (AWS, GCP, Azure, etc ) and assigns an external IP address of the service to it. It distributes incoming traffic to the service across the available pods. LoadBalancer services are typically used to expose services externally, and they work well in cloud environments.
- **ExternalName**: This type of service maps a service to an external DNS name. It does not provide any load balancing or proxying capabilities but allows accessing external services by name within the cluster. It is commonly used to integrate with external services outside the cluster in a service-like manner.
- **HeadlessService**: this type of service does not assign a cluster IP to the service itself. Normally, when you create a service in Kubernetes, it is assigned a virtual IP address that acts as a load balancer for pods and a single entry point to access the pods behind the service. However, with a headless service, there is no virtual IP allocated. Instead, the service is associated with a DNS record that points directly to the individual IP addresses of the pods that are part of the service. Each pod can be addressed individually by its IP or hostname. This allows direct communication with specific pods without load balancing or proxying. Headless services are commonly used in scenarios where you need to address individual pods directly, such as in distributed systems or database clusters. To create a headless service in Kubernetes, you can specify `clusterIP: None` in the service definition YAML file. This ensures that no cluster IP is assigned to the service, making it a headless service.

**Ingress** is a component responsible for enabling external access to the cluster services; it implements k8s *ingresscontroller* API. *Ingress Operator* manages one or more HAProxy-based *Ingress Controllers* to handle routing. You can configure *Ingress Operator* to route traffic by specifying OSCP *Route* and Kubernetes *Ingress* resources.

**Route** is an OSCP-specific *Ingress*. It exposes a *Service* at a host name, like [www.example.com](http://www.example.com/), so that external clients can reach it by name. It assigns a DNS to the *Service* and supports TLS termination (edge, passthrough, reencrypt), but DNS resolution is handled separately from the routing. Each route consists of a name (limited to 63 characters), a service selector, and an optional security configuration.

**Job** is a controller for one-time tasks. Job runs one or several pods and ensures their successful resolution. It can consume *ConfigMaps* and *Secrets*. For scheduled tasks *CronJob* exists.

**Template** – a set of objects that can be parameterized and processed to produce a list of objects for creation. A template can be processed to create anything you have permission to create within a project, for example *services*, *build* or *deployment* configurations. A template may also define a set of labels to apply to every object defined in the template.
- **Parameters** allow a value to be supplied by the user or generated when the template is instantiated. It may be specified when the template is created, or default will be used.
- **Labels** are used to organize, group, or select API objects. Most objects can include labels in their metadata. Labels can be used to group arbitrarily related objects; for example, all of the pods, services, replication controllers, and deployment configurations of a particular application can be grouped.
- **Objects** will be created when the template is instantiated. This list can include any valid API objects, such as *BuildConfig*, *DeploymentConfig*, *Service*, etc.

**CRD** stands for **Custom Resource Definition**. It is an extension mechanism that allows you to define your own custom resources in addition to the built-in resources provided by Kubernetes. CRDs enable to extend Kubernetes API and define new object types that can be managed and operated like native Kubernetes resources. With CRDs, you can define and create your own application-specific resources and controllers to manage those resources.

**Kubernetes Operator** is a k8s extension that extends the functionality of the Kubernetes API to create, configure and manage instances of complex applications on behalf of a Kubernetes user. It makes use of *CRD*s and builds upon the basic Kubernetes resource a complex stateful application by providing into the software operational application-specific knowledge to automate the entire life cycle of it. Operators serve as a packaging mechanism for distributing applications on Kubernetes, and they monitor, maintain, recover, and upgrade the software they deploy. [Operator.](https://www.redhat.com/en/topics/containers/what-is-a-kubernetes-operator)
**Prometheus Operator** is a Kubernetes operator designed to simplify the deployment and management of Prometheus and related monitoring components in a Kubernetes environment. It leverages the native Kubernetes API and extends the functionality of Prometheus by providing automated configuration and management capabilities. PO introduces *CRD*s to Kubernetes like *Prometheus*, *ServiceMonitor*, and *Alertmanager*. These CRDs allow users to define and manage Prometheus instances, monitor specific services, and configure alerting rules.
- **ServiceMonitors** define the monitoring targets for Prometheus. They specify which services or pods should be monitored and define scraping endpoints, intervals, timeouts, and relabeling rules. ServiceMonitors enable dynamic discovery and monitoring of services in the k8s cluster.
- **Alertmanager** handles alert notifications and routing. It allows users to define alerting rules and configure how alerts should be sent and managed. The operator automatically manages the deployment and configuration of Alertmanager instances.

**Service account** is an OpenShift Container Platform account that allows a component to directly access OS API. Service accounts are API objects that exist within each project. Service accounts provide a flexible way to control API access without sharing a regular user’s credentials. For example, service accounts can allow:
- Replication controllers to make API calls to create or delete pods.
- Applications inside containers to make API calls for discovery purposes.
- External applications to make API calls for monitoring or integration purposes.

[OpenShift API objects](https://docs.openshift.com/container-platform/3.11/rest_api/index.html)
