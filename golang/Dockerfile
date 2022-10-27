FROM golang:1.19-alpine as builder
RUN apk add bash
WORKDIR /app
COPY . .
RUN go build -ldflags "-s -w" -o server .
RUN chmod 777 server

FROM golang:1.19-alpine
WORKDIR /app
COPY --from=builder /app/server server
CMD [ "./server" ]
