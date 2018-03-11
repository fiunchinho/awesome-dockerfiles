FROM openjdk:8-jdk-alpine as builder

WORKDIR /app

COPY . /app
RUN ./gradlew clean build


FROM openjdk:8-jdk-alpine

LABEL maintainer="Jose Armesto <jose@armesto.net>"

EXPOSE 8080

ENTRYPOINT ["/sbin/tini", "--"]
CMD ["java", "-jar", "-XX:+UnlockExperimentalVMOptions", "-XX:+UseCGroupMemoryLimitForHeap", "-XX:MaxRAMFraction=2", "app.jar"]

WORKDIR /app

RUN apk add --update ca-certificates tini; \
    rm -rf /var/cache/apk/* /tmp/*

COPY --from=builder /app/build/libs/app.jar /app/app.jar
