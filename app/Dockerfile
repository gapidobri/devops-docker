FROM golang:1.25.5-alpine AS build

WORKDIR /app

COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build -o todoapp


FROM alpine:latest

WORKDIR /usr/local/todoapp

COPY --from=build /app/todoapp /usr/local/todoapp/todoapp
COPY --from=build /app/templates /usr/local/todoapp/templates
COPY --from=build /app/static /usr/local/todoapp/static

ENV TEMPLATES_PATH=/usr/local/todoapp/templates
ENV STATIC_PATH=/usr/local/todoapp/static

ENV GIN_MODE=release

ENTRYPOINT ["/usr/local/todoapp/todoapp"]