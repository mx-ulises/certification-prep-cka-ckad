# Stage 1: Build
FROM golang:1.21 as build
WORKDIR /src
COPY <<EOF /src/main.go
package main

import "fmt"

func main() {
  fmt.Println("hello, world")
}
EOF
RUN go build -o /bin/hello ./main.go

# SECURITY STEP: Create multistage image
# SECURITY STEP: Pick a specific label of the image
FROM alpine:3.14
# SECURITY STEP: Remove write permissions from the directories
RUN chmod a-w /etc
# SECURITY STEP: Add new user and group different from root
RUN addgroup -S app && adduser -S app -G app
# SECURITY STEP: Remove unneserary binaries, including shell access
RUN rm -rf /bin/*
COPY --from=build /bin/hello /bin/hello
# SECURITY STEP: Change user to non-root
USER app
CMD ["/bin/hello"]
