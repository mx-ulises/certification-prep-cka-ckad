# Docker immage security

Some steps to add security to the images, these are implemented in the `Dockerfile`:

 1. Implement Multi-stage images to have minimal images in size and in binaries by coping them from build stage:
    ```
    # Stage 1: Build image
    FROM golang:1.21 as build
    ...

    # Stage 2: Image with binary
    FROM alpine:3.14
    ...
    COPY --from=build /bin/hello /bin/hello
    CMD ["/bin/hello"]
    ```
 1. Use specific tags of the images:
    ```
    FROM alpine:3.14
    ```
 1. Remove write permissions from specific folders:
    ```
    FROM alpine:3.14
    RUN chmod a-w /etc
    ```
 1. Create a non-`root` user and use it:
    ```
    RUN addgroup -S app && adduser -S app -G app
    USER app
    ```
 1. Remove shell and other critical binaries that you don't need:
    ```
    RUN rm -rf /bin/*
    ```
