FROM golang:1.26.5-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /bin/server .

FROM alpine:3.22

RUN apk add --no-cache ffmpeg ca-certificates \
    && addgroup -S app \
    && adduser -S app -G app

COPY --from=builder /bin/server /usr/local/bin/server

USER app

EXPOSE 8080

ENTRYPOINT ["server"]
