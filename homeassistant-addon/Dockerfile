#################
# 1 Extract Stage
#################
FROM ghcr.io/dispatcharr/dispatcharr:latest AS dispatcharr-source

#################
# 2 Build Image
#################
ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.18
FROM $BUILD_FROM

# Set S6 wait time
ENV S6_CMD_WAIT_FOR_SERVICES=1 \
    S6_CMD_WAIT_FOR_SERVICES_MAXTIME=0 \
    S6_SERVICES_GRACETIME=0

# Install minimal dependencies
RUN apk add --no-cache \
    bash \
    curl \
    jq

# Copy root filesystem for Home Assistant add-on
COPY rootfs/ /

# Copy Dispatcharr binary from official image
COPY --from=dispatcharr-source /app/dispatcharr /app/dispatcharr

# Ensure binary is executable
RUN chmod +x /app/dispatcharr

# Create data directory with proper permissions
RUN mkdir -p /data && chown -R root:root /data

# Volumes
VOLUME ["/data"]

# Labels for Home Assistant
LABEL \
    io.hass.name="Dispatcharr" \
    io.hass.description="IPTV & stream management with Home Assistant integration" \
    io.hass.type="addon" \
    maintainer="Stainy3244"
