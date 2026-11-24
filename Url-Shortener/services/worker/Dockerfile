# --- Stage 1: Build the Go binary ---
FROM golang:1.23-alpine AS builder

WORKDIR /app

# Copy dependency file and source code
COPY go.mod ./
COPY . .

# Generate go.sum and download dependencies, then compile
RUN go mod tidy && CGO_ENABLED=0 GOOS=linux go build -o worker .

# --- Stage 2: Create the minimal runtime image ---
FROM alpine:3.21

WORKDIR /app
COPY --from=builder /app/worker .

EXPOSE 8090
CMD ["./worker"]
