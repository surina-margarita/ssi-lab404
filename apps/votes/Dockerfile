FROM dhi.io/golang:1.26-alpine@sha256:c5f8e669aa9656a010aeb1f3c00d12684d136cf598ddbf7c50409b6bda57f7f4 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o votes .

FROM scratch

COPY --from=builder --chown=1000:1000 /app/votes /votes

USER 1000:1000

EXPOSE 8081

ENTRYPOINT ["/votes"]