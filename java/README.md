# Download dependencies from private repositories
If you need to download dependencies from a private repository, you may need to pass variables to define environment variables to the build process.

You can do that adding build arguments to your Dockerfile

```bash
FROM openjdk:8-jdk-alpine as builder

ARG ARTIFACTORY_USER="non-defined"
ARG ARTIFACTORY_PWD="non-defined"
ARG WORKDIR="/app"

WORKDIR ${WORKDIR}

# rest of the Dockerfile
```

and then pass them to the docker build command

```bash
docker build \
	--build-arg ARTIFACTORY_USER="my-username" \
	--build-arg ARTIFACTORY_PWD="s3cr3tp4ssw0rd" \
	-t "my-awesome-application" .
```

# Using Spring Boot bash script to run your application
If you'd like to execute your Spring Boot application using the script that's generated using `installDist` instead of the jar file, you need some changes

```bash
FROM openjdk:8-jdk-alpine as builder

ARG WORKDIR="/app"

WORKDIR ${WORKDIR}

COPY build.gradle gradlew gradle.properties ${WORKDIR}/
COPY gradle ${WORKDIR}/gradle
# The task `gradle dependencies` will list the dependencies and download them as a side-effect.
RUN ./gradlew --no-daemon clean dependencies --configuration runtime
COPY . ${WORKDIR}
# Execute `installDist` to generate script
RUN ./gradlew --no-daemon installDist


FROM openjdk:8-jdk-alpine

LABEL maintainer="Jose Armesto <jose@armesto.net>"

ARG WORKDIR="/app"

WORKDIR ${WORKDIR}

EXPOSE 8080

ENTRYPOINT ["/sbin/tini", "--"]
CMD ["app/bin/app"]

ENV JAVA_OPTS "-XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap -XX:MaxRAMFraction=2"

RUN apk add --update ca-certificates tini=0.17.0-r0; \
    rm -rf /var/cache/apk/* /tmp/*

COPY --from=builder ${WORKDIR}/build/install/ ${WORKDIR}/
```
