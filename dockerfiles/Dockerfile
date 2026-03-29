FROM --platform=$BUILDPLATFORM golang:1.25-alpine AS builder

ARG TARGETOS
ARG TARGETARCH
ARG TARGETVARIANT
ARG VERSION=dev

WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=$TARGETOS GOARCH=$TARGETARCH GOARM=${TARGETVARIANT#v} \
    go build \
    -ldflags="-s -w -X github.com/containrrr/watchtower/internal/meta.Version=${VERSION}" \
    -o watchtower ./main.go

FROM --platform=$BUILDPLATFORM alpine:3.19.0 AS alpine
RUN apk add --no-cache ca-certificates tzdata

FROM scratch
LABEL "com.centurylinklabs.watchtower"="true"

COPY --from=alpine /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
COPY --from=alpine /usr/share/zoneinfo /usr/share/zoneinfo

EXPOSE 8080

COPY --from=builder /app/watchtower /

HEALTHCHECK CMD [ "/watchtower", "--health-check"]
ENTRYPOINT ["/watchtower"]
