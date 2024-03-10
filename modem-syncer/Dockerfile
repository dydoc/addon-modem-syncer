# https://developers.home-assistant.io/docs/add-ons/configuration#add-on-dockerfile
ARG BUILD_FROM
FROM $BUILD_FROM AS build-stage


# Set destination for COPY
WORKDIR /app

RUN \
     apt-get update && \
     apt-get install software-properties-common -y && \
     add-apt-repository ppa:longsleep/golang-backports &&\
     apt update  && \
     apt install golang-go -y

# Download Go modules

COPY app/ ./
#RUN go mod download

RUN CGO_ENABLED=0 GOOS=linux go build -o /main

# Run the tests in the container
FROM build-stage AS run-test-stage
RUN go test -v ./...

# Deploy the application binary into a lean image
FROM $BUILD_FROM AS build-release-stage

WORKDIR /

COPY --from=build-stage /main /main

ENTRYPOINT ["/main"]
