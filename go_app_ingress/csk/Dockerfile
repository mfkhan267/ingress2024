# Stage 1: Build the Go application
FROM golang:alpine3.20 as builder

# Set the Current Working Directory inside the builder container
WORKDIR /app

# Copy the Go Modules files and download dependencies
COPY go.* ./
RUN go mod download

# Copy the source code into the builder container
COPY . .

# Build the Go application
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o main .

# Stage 2: Create a minimal image with the built binary
FROM scratch

# Set the working directory inside the final container
WORKDIR /app

# Copy the compiled binary from the builder container
COPY --from=builder /app/main .

# Copy the image file from the builder container
COPY --from=builder /app/image.png .

# Expose port 8080 to the outside world
EXPOSE 8080

# Command to run the executable
CMD ["./main"]

