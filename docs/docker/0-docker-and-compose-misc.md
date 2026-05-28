# Docker and Compose - 00 - Misc

***

## Maven, Java

Mainly for Maven projects but maybe easily adapted for standard Java project.

### Project

From IntelliJ Java (Maven) project to a container hosted on a Raspberry.

Deployment process:

```text
(IntelliJ)
maven <project> package
Dockerfile-arm64v8 build image
(terminal)
docker login
docker push oldu73/msc:<project>_arm64v8_latest
(raspberry)
maybe stop/remove previous existing <project> container instance
docker login
sudo docker pull oldu73/msc:<project>_arm64v8_latest
sudo docker run -d -p <port>:<port> --name <container_name> oldu73/msc:<project>_arm64v8_latest
sudo docker container ps -a
sudo docker exec -it <container_name> sh
(container)
tail -1000f /var/log/<file>.log
```

Dockerfile-arm64v8:

```text
FROM arm64v8/openjdk:8-oraclelinux7
COPY ./target/project.jar /tmp
WORKDIR /tmp
EXPOSE <port>
ENTRYPOINT ["java", "-jar", "project.jar"]
```

### Debug, HotSwap

```text
(IntelliJ)
maven <project> package
Dockerfile run image // bind port 8080
```

Dockerfile:

```text
FROM openjdk:8
COPY ./target/<project>.jar /tmp
WORKDIR /tmp
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "-agentlib:jdwp=transport=dt_socket,address=8080,server=y,suspend=n", "<project>.jar"]
```

IntelliJ, Remote JVM Debug, configuration:

```text
Debugger mode: Attach to remote JVM
Transport: Socket
Host: localhost
Port: 8080
Command line arguments for remote JVM: -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8080
```

Run IntelliJ remote debug configuration.

Modify code, set breakpoint and then reload changed class.

By adding some system out you may observe live changes in container log console.

***

## Disk usage

```console
docker system df
```

Docker reclaim unused image space:

```console
docker image prune -a
```

Docker container size:

```console
docker ps --size
```

***

## Re-up a Single Service (Back-end) Without Restarting the Full Stack

Rebuild and restart a specific Docker Compose service (e.g., back-end)\
without impacting the rest of the running stack.

This is useful in development when: - Backend code changes -
Dependencies are updated - The Dockerfile is modified - You want to
refresh only one service:

``` bash
docker compose up -d --build --force-recreate service
```

***

## Compose with profile

Docker Compose profiles allow you to define optional services that are only started when needed.
This is useful to keep the default environment lightweight while still being able to enable extra tools for debugging, monitoring, testing, or administration.

Example with two fake optional services:

```yaml
services:

  app:
    image: my-app

  database:
    image: postgres

  # Optional admin UI
  admin-ui:
    profiles: ["admin"]
    image: fake-admin-ui

  # Optional monitoring stack
  metrics-dashboard:
    profiles: ["monitoring"]
    image: fake-monitor-dashboard
```

Usage examples:

```bash
# Start only the core stack
docker compose up -d

# Start core stack + admin tools
docker compose --profile admin up -d

# Start core stack + monitoring tools
docker compose --profile monitoring up -d

# Start everything
docker compose --profile admin --profile monitoring up -d
```

Typical use cases for profiles:

- Enable heavy tools only when needed  
- Separate developer/debug services from production-like services  
- Provide optional UIs or dashboards  
- Reduce memory and startup time by default  
- Allow different team members to run only the tools they need

Without profiles, all services start automatically.
With profiles, optional services become opt-in.

***
