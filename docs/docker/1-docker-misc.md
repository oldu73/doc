# Docker - 01 - Misc

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
