#################
# 1 Build Image #
#################
ARG BUILD_FROM
FROM $BUILD_FROM

# Set S6 wait time
ENV S6_CMD_WAIT_FOR_SERVICES=1 \
    S6_CMD_WAIT_FOR_SERVICES_MAXTIME=0 \
    S6_SERVICES_GRACETIME=0

# Install dependencies
RUN apk add --no-cache \
    bash \
    curl \
    jq

# Copy root filesystem for HA add-on
COPY rootfs /

# Download and install Dispatcharr
RUN mkdir -p /app \
    && case "$TARGETPLATFORM" in \
        "linux/amd64") ARCH="amd64" ;; \
        "linux/arm64") ARCH="arm64" ;; \
        "linux/arm/v7") ARCH="armv7" ;; \
        *) echo "Unsupported platform $TARGETPLATFORM" && exit 1 ;; \
    esac \
    && DISPATCHARR_VERSION=$(curl -s "https://api.github.com/repos/dispatcharr/dispatcharr/releases/latest" | jq -r '.tag_name') \
    && curl -L "https://github.com/dispatcharr/dispatcharr/releases/download/${DISPATCHARR_VERSION}/dispatcharr_${DISPATCHARR_VERSION}_Linux_${ARCH}.tar.gz" \
    | tar xz -C /app \
    && chmod +x /app/dispatcharr

# Create data directory with proper permissions
RUN mkdir -p /data && chown -R root:root /data

# Volumes
VOLUME [ "/data" ]

# Labels
LABEL \
    io.hass.name="Dispatcharr" \
    io.hass.description="IPTV & stream management with Home Assistant integration" \
    io.hass.type="addon" \
    maintainer="Stainy3244"
