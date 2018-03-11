FROM golang:alpine as builder

WORKDIR /app

COPY . /app
RUN go build -o goapp


FROM alpine:3.7

LABEL maintainer="Jose Armesto <jose@armesto.net>"

EXPOSE 8080

ENTRYPOINT ["/sbin/tini", "--"]
CMD ./app/goapp

WORKDIR /app

RUN apk add --update ca-certificates tini=0.17.0-r0; \
    rm -rf /var/cache/apk/* /tmp/*

COPY --from=builder /app/goapp /app/
