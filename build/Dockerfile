FROM golang:1.11 AS build
WORKDIR /go/src/github.com/gitzup/gcp/
RUN curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh
COPY ./Gopkg.lock ./Gopkg.toml ./
RUN dep ensure -v -vendor-only
COPY ./api ./api/
COPY ./cmd ./cmd/
COPY ./internal ./internal/
COPY ./main.go ./Makefile ./
RUN make

########################################################################################################################
# NOTES:
#   - installing "ca-certificates" & calling "update-ca-certificates" so Pub/Sub won't hang when connecting via 443/SSL
########################################################################################################################

FROM ubuntu:18.04
RUN apt-get update && apt-get install -y ca-certificates && rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY --from=build /go/src/github.com/gitzup/gcp/gcp .
ENTRYPOINT ["/app/gcp"]
