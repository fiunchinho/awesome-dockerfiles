FROM openjdk:8-jdk-alpine as builder

ARG WORKDIR="/app"

WORKDIR ${WORKDIR}

COPY build.gradle gradlew gradle.properties ${WORKDIR}/
COPY gradle ${WORKDIR}/gradle
# The task `gradle dependencies` will list the dependencies and download them as a side-effect.
RUN ./gradlew --no-daemon clean dependencies
COPY . ${WORKDIR}
RUN ./gradlew --no-daemon build --exclude-task test


FROM openjdk:8-jdk-alpine

LABEL maintainer="Jose Armesto <jose@armesto.net>"

ARG WORKDIR="/app"

WORKDIR /app

EXPOSE 8080

ENTRYPOINT ["/sbin/tini", "--"]
CMD ["java", "-jar", "-XX:+UnlockExperimentalVMOptions", "-XX:+UseCGroupMemoryLimitForHeap", "-XX:MaxRAMFraction=2", "app.jar"]

RUN apk add --update ca-certificates tini=0.17.0-r0; \
    rm -rf /var/cache/apk/* /tmp/*

COPY --from=builder ${WORKDIR}/build/libs/app.jar app.jar
