# syntax=docker/dockerfile:1

##
## Build
##
FROM golang:1.18 AS build

WORKDIR /workdir

COPY go.mod ./
COPY go.sum ./
RUN go mod download

COPY ./ ./

RUN go build -o /app /workdir/cmd/alertmanager/main.go

##
## Deploy
##
FROM gcr.io/distroless/base-debian11

WORKDIR /

COPY --from=build /app /app
COPY examples/ha/alertmanager.yml /alertmanager.yml

EXPOSE 9093

ENTRYPOINT ["/app"]
