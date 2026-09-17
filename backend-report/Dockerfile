# -- Образ для сборки
FROM golang:1.27.1-alpine3.24 AS build

WORKDIR /app

RUN apk add --no-cache git

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
  go build -a -installsuffix cgo -o main .

# -- Runtime образ
FROM alpine:3.21.7

WORKDIR /app

RUN addgroup -S -g 65532 nonroot && adduser -S -u 65532 -G nonroot nonroot

COPY --from=build /app/main .

USER 65532:65532

EXPOSE 8081

CMD ["./main"]
