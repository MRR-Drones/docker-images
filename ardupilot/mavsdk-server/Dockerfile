# Debian-based MAVSDK Server Container
FROM debian:bookworm-slim

ARG MAVSDK_VERSION=v3.17.1

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    wget \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Download and install MAVSDK server
# Using the specified MAVSDK version
RUN wget -q https://github.com/mavlink/MAVSDK/releases/download/${MAVSDK_VERSION}/mavsdk_server_musl_x86_64 -O /usr/local/bin/mavsdk_server && \
    chmod +x /usr/local/bin/mavsdk_server

# Create a non-root user for running the server
RUN useradd -m -s /bin/bash mavsdk

# Switch to non-root user
USER mavsdk

# Expose default MAVSDK server port (gRPC)
EXPOSE 50051

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:50051 || exit 1

# Start MAVSDK server
# Server listens on 0.0.0.0:50051 by default

ENV MAV_INPUT_URL=udp://127.0.0.1:14550

ENTRYPOINT mavsdk_server -s ${MAV_INPUT_URL}

