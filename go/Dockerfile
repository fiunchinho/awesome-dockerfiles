FROM golang:alpine as builder

ARG WORKDIR="/app"

WORKDIR /app

COPY . ${WORKDIR}
RUN go build -o goapp


FROM alpine:3.7

LABEL maintainer="Jose Armesto <jose@armesto.net>"

ARG WORKDIR="/app"

WORKDIR ${WORKDIR}

EXPOSE 8080

ENTRYPOINT ["/sbin/tini", "--"]
CMD ["./app/goapp"]

RUN apk add --update ca-certificates tini=0.17.0-r0; \
    rm -rf /var/cache/apk/* /tmp/*

COPY --from=builder ${WORKDIR}/goapp ${WORKDIR}
