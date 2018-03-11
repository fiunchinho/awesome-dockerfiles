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

RUN apk --no-cache add ca-certificates

COPY --from=builder /app/goapp /app/
