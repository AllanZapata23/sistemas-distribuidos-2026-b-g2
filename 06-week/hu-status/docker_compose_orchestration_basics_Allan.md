# Docker Compose and Container Orchestration Basics

## 1. Introduction

Modern applications are rarely made up of a single program running on its own. They often consist of several services that work together. For example, a web application may need:

- **Frontend:** the interface users interact with.
- **Backend or API:** processes requests and contains the application's business logic.
- **Database:** stores information.
- **Cache:** temporarily stores frequently requested data.
- **Message queue:** helps coordinate tasks between services.
- **Monitoring tools:** help observe system health and performance.

Managing each component separately can become complicated. Containers must be started, configured, connected through a network, and checked to ensure they are working correctly. Important data must also be preserved.

Two concepts help address these challenges:

- **Docker Compose:** defines and manages multiple related containers from a single configuration file. It is especially useful for development teams and small environments running on one machine.
- **Container orchestration:** automates the deployment, communication, scaling, and recovery of containerized applications. Platforms such as Kubernetes are designed to manage applications in more complex environments across multiple servers.

These technologies complement each other: Docker Compose simplifies local workflows, while an orchestration platform automates operations at a larger scale.

---

## 2. What Is Docker Compose?

Docker Compose is a tool for defining and managing applications that use multiple Docker containers. The configuration is usually written in a file named `compose.yaml` or `docker-compose.yml`.

Instead of running a separate command to start each container, you describe the services in the configuration file and start them together with a command such as:

```bash
docker compose up
```

For example, an application may include a web service, an API, and a database. Compose lets you specify which image each service uses, which ports are published, which environment variables it needs, and what storage should be retained.

### 2.1. Why Does Docker Compose Exist?

Suppose an application needs four components:

1. A frontend application.
2. A backend API.
3. A MySQL database.
4. A Redis cache.

Without Compose, a developer would need to create, configure, start, and connect each container individually. They would also need to remember all the parameters required to repeat the process.

With Compose, these services are declared in one file and the environment can be started with:

```bash
docker compose up
```

This simplifies environment setup, reduces repetitive commands, and makes it easier for other team members to use a similar configuration.

---

## 3. Main Benefits of Docker Compose

### 3.1. Simplifies Multi-Container Deployments

All services in an application can be described in a single configuration file. Instead of running many `docker run` commands, you can use `docker compose up`.

This helps you:

- Start related services in a coordinated way.
- Keep configuration in one place.
- Repeat the setup and execution process.
- Reduce errors caused by different manual configurations.

### 3.2. Improves Environment Consistency

When team members use the same Compose file, it is easier for them to work with equivalent services and configurations.

This reduces the familiar “it works on my machine” problem, which occurs when an application behaves differently because of differences between environments.

Benefits include:

- **Reproducible environments:** a known configuration can be recreated.
- **Easier onboarding:** team members share the same configuration file.
- **Fewer differences between machines:** fewer manual adjustments are required.

Reproducibility also depends on factors such as image versions, configuration files, and external data.

### 3.3. Automates Networking Between Services

Docker Compose creates a network for the project by default. Containers can communicate using service names, without manually configuring a fixed IP address for every container.

Example:

```yaml
services:
  web:
    image: nginx

  database:
    image: mysql
```

Within the Compose network, the `web` service can reach the `database` service using the hostname `database` and the appropriate port. For example, `database:3306` is the usual address for connecting to MySQL.

This simplifies communication and avoids depending on IP addresses that may change when containers are recreated.

### 3.4. Supports Environment Variables

Environment variables let you configure an application without changing its source code. They can specify database names, connection settings, execution modes, and other options.

Illustrative example:

```yaml
services:
  database:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
```

In this example, the password is supplied through a variable defined in the environment or through a compatible configuration mechanism. It is preferable not to write real passwords directly into a file that is shared or committed to version control.

Environment variables make configuration easier, but **they are not, by themselves, a secure secret-management system**. Important credentials should be handled using appropriate mechanisms, access should be restricted, and values should not be published.

### 3.5. Preserves Data with Volumes

Containers can be stopped, removed, and recreated. If important data exists only in a container's writable layer, it may be lost when that container is removed.

Volumes allow data to be stored separately from a container's lifecycle. They are especially important for databases.

Example:

```yaml
services:
  database:
    image: mysql:8.0
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

In this example, `mysql_data` is a Docker-managed volume mounted at the location where MySQL stores its data.

Benefits include:

- Data persistence when a container is recreated.
- Easier organization of backups.
- Separation of data lifecycle from container lifecycle.

A volume does not replace a backup policy. Appropriate backups should be created and tested to protect information.

### 3.6. Makes It Easy to Reset an Environment

If you need to stop and recreate the environment, you can use:

```bash
docker compose down
docker compose up -d
```

The first command stops and removes the project's containers and the network created by Compose. The second recreates and starts the services in the background.

By default, `docker compose down` does not remove named volumes. However, adding `-v` removes the Compose-managed volumes associated with the project, which can delete persistent data. Use that option carefully.

---

## 4. Basic Structure of a Docker Compose File

A Compose file describes the services and their settings. Here is an educational example:

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"

  database:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

This file defines two services:

- `web`: uses the Nginx image and publishes container port 80 on host port 8080.
- `database`: uses MySQL, receives its password through an environment variable, and stores its data in the `mysql_data` volume.

For this example, `MYSQL_ROOT_PASSWORD` must be defined before starting the project. In a real environment, follow MySQL security and configuration recommendations.

### 4.1. `services`

The `services` section declares the components that make up the application. Each service describes how a component should run, such as a web server, API, or database.

```yaml
services:
  web:
    image: nginx:latest
```

Here, `web` is the service name. Other services can use this name to communicate with it on the Compose network.

### 4.2. `image`

The `image` property specifies which container image should be used.

```yaml
image: nginx:latest
```

If the image is not available locally, Docker will normally try to download it from the relevant registry. The `latest` tag is a registry tag, but it does not guarantee that the image contents will remain identical forever. For more reproducible deployments, choose specific versions and manage updates deliberately.

### 4.3. `ports`

The `ports` property publishes a container port on the host machine.

```yaml
ports:
  - "8080:80"
```

The usual format is:

```text
HOST_PORT:CONTAINER_PORT
```

Therefore, `"8080:80"` means that container port 80 is published through host port 8080. If the web service is listening correctly on port 80, you can try accessing it from the host at `http://localhost:8080`.

Publishing a port may make a service accessible from other network interfaces, depending on the configuration. Do not publish sensitive services without reviewing exposure and access rules.

### 4.4. `environment`

The `environment` property defines environment variables that the container receives.

```yaml
environment:
  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
```

These variables can set passwords, runtime options, service addresses, and other parameters. For sensitive values, use an appropriate secret-management system rather than storing real credentials in shared repositories.

### 4.5. `volumes`

The `volumes` property mounts storage into a container.

```yaml
volumes:
  - mysql_data:/var/lib/mysql
```

The path on the right is the location inside the container. `mysql_data` identifies the persistent volume. A named volume can be declared at the top level of the file:

```yaml
volumes:
  mysql_data:
```

This storage helps preserve information even when the container is removed and recreated.

### 4.6. `version`

Some older files include a property such as:

```yaml
version: "3.9"
```

In modern versions of Docker Compose, the top-level `version` field is obsolete and can normally be omitted. Compose validates the file using the specification it implements. Newer examples often begin directly with `services:`.

---

## 5. Common Docker Compose Commands

Run these commands in a terminal opened in the directory containing the Compose file.

### 5.1. Start All Services

```bash
docker compose up
```

This creates containers and networks defined by the project, builds or downloads required images when appropriate, and starts the services. Container output is normally displayed in the terminal.

### 5.2. Start in the Background

```bash
docker compose up -d
```

The `-d` option means *detached*. It runs containers in the background and returns control of the terminal.

This is useful when you want to keep using the terminal without displaying service output in the foreground.

### 5.3. Stop and Remove Containers

```bash
docker compose down
```

This stops and removes the project's containers and networks created by Compose. By default, it does not remove named volumes.

The alternative:

```bash
docker compose down -v
```

also removes the associated Compose-managed volumes. **This can permanently delete persistent data**, so use it only when you intend to remove that data.

### 5.4. Rebuild Images

```bash
docker compose up --build
```

This requests a build of images that have a build configuration, such as a `build:` section. It is useful after modifying source code or a service's Dockerfile.

If a service uses only a prebuilt image specified with `image:`, `--build` does not automatically create a new version of that image from local source code.

### 5.5. View the Project's Containers

```bash
docker compose ps
```

This displays information about the project's containers, such as their names, status, and published ports. It helps you check whether services are running.

### 5.6. View Logs

```bash
docker compose logs
```

This displays service logs. To inspect one service:

```bash
docker compose logs web
```

Logs help investigate startup errors, connection problems, and other application behavior. To follow new log entries as they appear, use:

```bash
docker compose logs -f
```

### 5.7. Restart Services

```bash
docker compose restart
```

This restarts the services defined in the project. Restarting is not the same as rebuilding an image. If code or an image has changed, you may need to rebuild or recreate the containers.

### 5.8. Stop Without Removing Containers

```bash
docker compose stop
```

This stops the project's containers but does not remove them. You can start them again with `docker compose start` or another appropriate command.

### 5.9. Command Summary

| Goal | Command | What It Does |
|---|---|---|
| Start services | `docker compose up` | Creates and starts services, displaying output in the terminal. |
| Start in the background | `docker compose up -d` | Starts services without keeping them in the foreground. |
| Stop and remove containers | `docker compose down` | Removes project containers and networks; normally preserves named volumes. |
| Also remove volumes | `docker compose down -v` | Also removes associated volumes and may delete data. |
| Build before starting | `docker compose up --build` | Builds images with build configurations and starts services. |
| Check status | `docker compose ps` | Shows the status of the project's containers. |
| View logs | `docker compose logs` | Displays service output. |
| Restart | `docker compose restart` | Restarts services. |
| Stop without removing | `docker compose stop` | Stops containers and keeps them. |

---

## 6. What Is Container Orchestration?

Container orchestration is the automated management of applications that run in containers. An orchestration platform can coordinate deployment, networking, the number of instances, request distribution, monitoring, and service recovery.

For a small application, containers may be managed manually. As the system grows, the number of components and machines can make manual management slow and error-prone.

An orchestration platform lets you declare the desired state of an application. For example, you can specify that a web application should have three active instances. The platform attempts to maintain that state and replace failed instances according to its configuration.

### 6.1. Why Is It Important?

Imagine an application with hundreds of containers, several databases, multiple environments, and thousands of users. A team would need to monitor each service, respond to failures, adjust resources, and distribute requests.

Orchestration automates many of these tasks and helps manage large systems more consistently.

---

## 7. Problems Solved by Orchestration

### 7.1. Managing Multiple Services

Modern applications may include APIs, databases, authentication services, message queues, and monitoring tools. These components must communicate reliably.

Orchestration helps deploy the components and configure communication between them according to the platform's rules.

### 7.2. Recovering from Failures

A container may stop working because of software errors, insufficient memory, or other problems. An orchestration platform can detect certain failures and restart or replace instances to restore the desired state.

Recovery depends on health checks, policies, and system configuration. It does not mean every error is automatically fixed or that interruptions can never occur.

### 7.3. Scaling During Changes in Demand

Application traffic can vary throughout the day. For example, a service might need three replicas in the morning and fifteen during a period of high demand.

An orchestration platform can support manual or automatic scaling based on resources, metrics, and configured policies. Automatic scaling requires configuration and sufficient available resources.

### 7.4. Managing Updates

Applications are updated frequently. Some platforms support gradual updates (*rolling updates*), deploying a version to a subset of users (*canary deployments*), and returning to a previous version (*rollbacks*) when the platform and configuration support those operations.

These techniques help reduce risk, but they still require testing, monitoring, and an appropriate strategy.

### 7.5. Distributing Requests

When several instances of a service are running, requests can be distributed among them through load-balancing mechanisms. This helps prevent one instance from receiving all the traffic, improves responsiveness, and may increase availability.

The outcome depends on the architecture, instance health, and network configuration.

---

## 8. Key Orchestration Concepts

### 8.1. Deployment

A deployment defines how an application should run. For example, you might specify that a web application should maintain three replicas.

```text
Web application
Desired replicas: 3
        |
        v
The platform maintains the configured state
```

The platform compares the current state with the desired state. If an instance fails, it may attempt to replace it to restore the configured number.

Benefits:

- More consistent deployments.
- Centralized instance management.
- Automated recovery when configured.

In Kubernetes, a `Deployment` manages replicas of a stateless application through related objects such as `ReplicaSets` and `Pods`.

### 8.2. Scaling

Scaling adjusts an application's capacity to meet its needs.

**Horizontal scaling:** adds or removes instances.

```text
2 replicas  ->  10 replicas
```

This can increase request-handling capacity and reduce dependence on a single instance. The application and its dependencies must be able to work with multiple instances.

**Vertical scaling:** increases or decreases resources assigned to an instance, such as CPU or memory.

```text
More CPU
More memory
More available resources
```

Vertical scaling has physical limits and depends on platform support. Horizontal and vertical scaling can be combined.

### 8.3. Load Balancing

Load balancing distributes requests among multiple available instances.

Without load balancing, a single instance might receive too many requests and become a bottleneck.

```text
User requests
      |
      v
Load balancer
   /   |   \\
  v    v    v
Instance A  Instance B  Instance C
```

Benefits include more balanced traffic distribution, better responsiveness, and improved tolerance of certain failures. The load balancer must know or detect which instances are available.

### 8.4. Self-Healing

Self-healing is the ability to detect certain unhealthy conditions and attempt to recover the service automatically.

```text
An instance fails
        |
        v
The platform detects the problem
        |
        v
The instance is restarted or replaced
```

This can reduce the need for manual intervention and improve stability. It does not necessarily fix the original cause of a failure: if the problem persists, the replacement instance may fail too.

### 8.5. Service Discovery

Services need to find other components in order to communicate. Instead of relying on a fixed IP address, they can use service names or discovery mechanisms provided by the platform.

Examples of names:

```text
database
redis
backend-api
```

In a Docker Compose network, services are generally resolved by service name. In Kubernetes, service discovery uses mechanisms such as `Service` objects and cluster DNS.

This makes infrastructure changes easier because instances can be recreated and their IP addresses can change.

### 8.6. Monitoring

Monitoring means observing an application's and its infrastructure's state and performance. It may include CPU and memory usage, availability, errors, response times, and logs.

Monitoring helps detect problems, investigate their causes, and verify whether corrective actions work. A platform may provide health checks, but a complete monitoring system usually requires additional tools and configuration.

---

## 9. Docker Compose vs. Kubernetes

Docker Compose and Kubernetes both manage containerized applications, but they are designed for different needs.

| Feature | Docker Compose | Kubernetes |
|---|---|---|
| Learning curve | Generally simple | More advanced |
| Initial setup | Relatively simple | Requires a cluster or a managed cluster service |
| Common use | Local development, testing, and small environments | Production and distributed systems, depending on requirements |
| Scaling | Can manage multiple instances, with more limited capabilities | Provides extensive scaling mechanisms |
| Self-healing | Limited capabilities; it does not provide the same cluster-level reconciliation as an orchestrator | Can replace or restart workloads according to configuration |
| Load balancing | Networking and port publishing; other capabilities may require additional services | Provides networking and service abstractions, with details depending on configuration |
| High availability | Does not, by itself, coordinate a multi-node high-availability cluster | Can be designed for high availability with multiple nodes and redundant components |
| Multiple nodes | Not intended to manage an application as a multi-node cluster | Designed to coordinate workloads across a cluster of nodes |
| Operational complexity | Lower for small projects | Higher because of additional components and configuration |
| Enterprise use | Useful for development and some small deployments | Common in enterprise platforms and distributed systems |

This is a general comparison. Specific capabilities depend on versions, configuration, and additional tools. Kubernetes does not guarantee high availability by itself; architecture and operations must also be designed appropriately.

### 9.1. When Should You Use Docker Compose?

It is often suitable when:

- Developing an application on a single computer.
- Starting a frontend, backend, and database together.
- Reproducing test environments.
- Looking for a simple configuration for a small project.

### 9.2. When Should You Use Kubernetes?

It may be suitable when:

- An application needs to run across multiple nodes.
- Advanced deployment and scaling mechanisms are required.
- Services need to remain available despite certain failures.
- The organization has the experience and resources to operate a cluster.

Kubernetes adds complexity, so it is not always necessary for a small application.

---

## 10. Typical Workflow

### 10.1. Development Environment

A typical Docker Compose workflow looks like this:

```text
Developer
    |
    v
Docker Compose
    |
    +--> Frontend container
    |
    +--> Backend container
    |
    +--> Database container
```

The developer defines the services in the Compose file and uses commands to start, inspect, stop, and rebuild the environment.

Benefits:

- Fast environment setup.
- Local testing with multiple services.
- Application debugging.
- Shared configuration across team members.

### 10.2. Production Environment

A possible workflow in an environment using Kubernetes is:

```text
Developer
    |
    v
Build container images
    |
    v
Image registry
    |
    v
Kubernetes cluster
    |
    v
Deployment and monitoring
    |
    v
Scaling and recovery according to configuration
```

Images are built and published to a registry. The cluster retrieves the images and runs the declared workloads. The platform helps manage instances, communication, and the desired state.

The exact architecture depends on the project. Not every system needs Kubernetes, and a production deployment also requires security, backups, observability, secret management, and an update process.

---

## 11. Best Practices

### 11.1. Separate Environments

It is useful to distinguish between:

- **Development:** where code is written and initial tests are performed.
- **Testing:** where changes are verified.
- **Production:** where the application used by its users runs.

Separating environments helps reduce the risk of a test affecting real data or services. It also allows different permissions and configurations for each environment.

### 11.2. Store Secrets Securely

Avoid writing real passwords directly into files that are stored in shared repositories.

Options include:

- Docker Secrets, where supported by the environment and tool.
- Kubernetes Secrets.
- Dedicated secret managers such as Azure Key Vault, AWS Secrets Manager, or HashiCorp Vault.

The chosen tool must be configured with appropriate permissions. For example, Kubernetes Secrets require suitable security configuration and should not be considered protected merely because they exist as Kubernetes objects.

### 11.3. Use Persistent Storage

Databases should store their information in suitable storage rather than relying only on a container's temporary writable layer.

Example:

```yaml
services:
  database:
    image: mysql:8.0
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

In addition to using volumes, establish backup, retention, and recovery policies. Persistence helps preserve data but does not, by itself, protect against every incident.

### 11.4. Define Resource Limits

Containers can consume too much CPU or memory if they are not managed appropriately. Resource limits help prevent one service from excessively affecting others.

Illustrative resource configuration:

```yaml
services:
  web:
    image: nginx:latest
    deploy:
      resources:
        limits:
          cpus: "1"
          memory: 512M
```

Support for and the exact effect of resource options depend on the implementation and runtime environment. Verify the configuration being used and choose limits that match the application's needs.

### 11.5. Keep Images Lightweight and Updated

Smaller images are usually faster to download and take up less storage. Removing unnecessary components can also reduce the attack surface.

Example:

```dockerfile
FROM alpine:latest
```

Alpine is a lightweight base image, but it is not automatically the best choice for every application. The image must be compatible with the software, kept up to date, and obtained from a trusted source. Avoid relying on tags that may change unexpectedly, and review image vulnerabilities.

### 11.6. Configure Health Checks

Health checks help determine whether a service responds as expected.

Illustrative example:

```yaml
services:
  web:
    image: nginx:latest
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost"]
      interval: 30s
      timeout: 5s
      retries: 3
```

This example requires the `curl` command to be available in the image. If it is not available, the check will fail and you must choose a test that is compatible with the container.

A health check should verify something meaningful for the service. A running process does not always mean the application is ready to handle requests.

### 11.7. Apply Infrastructure as Code (IaC)

Infrastructure as Code means describing infrastructure and configuration in files that can be versioned, reviewed, and shared.

Examples include:

- Docker Compose files.
- Kubernetes YAML manifests.
- Terraform configurations.

Benefits:

- **Reproducibility:** helps recreate environments.
- **Traceability:** makes it possible to review changes over time.
- **Collaboration:** teams can share and review configurations.
- **Auditing:** helps establish what changed and when.

Version control should be combined with reviews, appropriate permissions, and secure credential management.

---

## 12. Integrated Practical Example

The following example brings together several Compose concepts: services, ports, environment variables, networking, and persistent volumes.

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    depends_on:
      - database

  database:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: app_db
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

### How Should You Read This Example?

1. **`services`:** declares the application's components.
2. **`web`:** runs Nginx and publishes container port 80 on host port 8080.
3. **`database`:** runs MySQL and creates the database specified by `MYSQL_DATABASE`, according to the behavior of the official image.
4. **`environment`:** provides configuration values to MySQL. The `MYSQL_ROOT_PASSWORD` variable must be defined before the project starts.
5. **`volumes`:** preserves MySQL data in the `mysql_data` volume.
6. **`depends_on`:** establishes a startup dependency between the services. By itself, it does not guarantee that MySQL is ready to accept connections when Nginx starts.

This is an educational example: Nginx does not automatically become an application connected to MySQL. For a working application, a backend would need to be configured to use the database, and the appropriate images and parameters would be required.

### Commands for This Example

```bash
# Start the services
docker compose up -d

# Check their status
docker compose ps

# View logs
docker compose logs

# Stop and remove the project's containers and networks
docker compose down
```

If you also want to remove the volume and the data stored in it, you can use `docker compose down -v`, but only after confirming that the data can be deleted.

---

## 13. Recommended Learning Path

A common progression for learning these technologies is:

```text
Docker
  |
  v
Docker Compose
  |
  v
Container Networking and Storage
  |
  v
Kubernetes
  |
  v
Cloud-Native Architecture
```

### 13.1. Docker

Learn what images and containers are, how to run a container, how to inspect its logs, and how to build an image with a Dockerfile.

### 13.2. Docker Compose

Practice defining multiple services in one file and using networks, environment variables, volumes, and the main commands.

### 13.3. Networking and Storage

Understand how services communicate, what it means to publish a port, how name resolution works, and how data can be stored outside a container.

### 13.4. Kubernetes

Learn the concepts of clusters, nodes, Pods, Deployments, Services, configuration, secrets, health checks, and scaling.

### 13.5. Cloud-Native Architecture

Explore automated deployments, observability, security, resource management, and continuous delivery practices.

---

## 14. Conclusion

Docker Compose and container orchestration solve related but different problems.

**Docker Compose** lets you define and run multiple containers from a centralized configuration. It is straightforward to use and especially useful for development, testing, and small environments. Its services can share networks, use environment variables, and preserve data through volumes.

**Container orchestration**, through platforms such as **Kubernetes**, is designed to manage applications in more complex infrastructure. It can automate deployments, maintain the desired state, facilitate scaling, distribute requests, discover services, and recover certain workloads after failures.

Choosing a tool depends on the requirements. Not every application needs a Kubernetes cluster, and using an orchestrator does not eliminate the need to design security, backups, monitoring, and operational processes correctly.

Learning Docker, Docker Compose, networking, storage, and then Kubernetes provides a strong foundation for DevOps, microservices, and cloud-native applications.

---

## 15. Glossary

| Term | Explanation |
|---|---|
| **Container** | An isolated environment in which an application runs with its dependencies. |
| **Image** | A template used to create containers. |
| **Service** | An application component described in Compose or managed by a platform. |
| **Docker Compose** | A tool for defining and running multi-container applications. |
| **Orchestration** | Automation of container deployment and operations. |
| **Volume** | Storage that can persist beyond a container's lifecycle. |
| **Environment variable** | A configuration value available to a process through its environment. |
| **Published port** | A host port mapped to a port inside a container. |
| **Replica** | An additional instance of a workload or application. |
| **Horizontal scaling** | Increasing or decreasing the number of instances. |
| **Vertical scaling** | Increasing or decreasing resources assigned to an instance. |
| **Load balancing** | Distributing requests across instances. |
| **Self-healing** | Mechanisms that detect certain failures and attempt to recover a service. |
| **Service discovery** | Mechanisms for locating services and communicating with them. |
| **Kubernetes** | A container orchestration platform. |
| **Infrastructure as Code (IaC)** | Describing infrastructure through configuration files that can be versioned. |
| **Image registry** | A service for storing and distributing container images. |
| **Monitoring** | Observing a system's health, availability, and performance. |

---

## 16. Review Questions

1. What problem does Docker Compose solve?
2. What information is declared in a `compose.yaml` file?
3. What is the difference between an image and a container?
4. What does the port mapping `"8080:80"` mean?
5. Why are volumes important for a database?
6. What is the difference between `docker compose stop` and `docker compose down`?
7. What risk is associated with `docker compose down -v`?
8. What is container orchestration?
9. What is the difference between horizontal and vertical scaling?
10. How does load balancing help?
11. What does self-healing mean in an orchestration platform?
12. What are the general differences between Docker Compose and Kubernetes?
13. Why should real passwords not be stored directly in a repository?
14. What are health checks used for?
15. What does Infrastructure as Code mean?

### Short Answers

1. It allows multiple containers to be defined and managed as one application.
2. Services, images, ports, environment variables, networks, volumes, and other settings.
3. An image is the template; a container is an instance created from it.
4. It publishes container port 80 through host port 8080.
5. They preserve data even when a container is removed or recreated.
6. `stop` stops containers; `down` also removes the project's containers and networks.
7. It can remove associated volumes and delete persistent data.
8. It is the automated management of applications made up of containers.
9. Horizontal scaling changes the number of instances; vertical scaling changes the resources assigned to an instance.
10. It distributes requests among available instances.
11. The platform detects certain failures and attempts to restore the desired state.
12. Compose is simpler and common in local development; Kubernetes manages workloads in clusters and offers broader orchestration capabilities.
13. They could be exposed to anyone with access to the repository.
14. They check whether a service is healthy or ready according to the configured test.
15. It means describing infrastructure in files that can be versioned and reviewed.
