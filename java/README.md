# Download dependencies from private repositories
If you need to download dependencies from a private repository, you may need to pass variables to the build process.

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
