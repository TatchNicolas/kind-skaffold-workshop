FROM golang:latest as builder

ENV CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64 \
    GO111MODULE=on

WORKDIR /opt/app
COPY . /opt/app
RUN go build -o main


# runtime image
FROM jrottenberg/ffmpeg:3.2-alpine
COPY --from=builder /opt/app/main /opt/app/main
ENTRYPOINT []
CMD /opt/app/main
