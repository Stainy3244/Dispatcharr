#################
# 1 Build Image #
#################

ARG BUILD_FROM
FROM $BUILD_FROM as buildstage

# Set S6 wait time
ENV S6_CMD_WAIT_FOR_SERVICES=1 \
    S6_CMD_WAIT_FOR_SERVICES_MAXTIME=0 \
    S6_SERVICES_GRACETIME=0

# Install packages
RUN apk add --no-cache \
        bash \
        curl \
        jq

################
# 2 Final Image #
################

FROM buildstage

# Set S6 wait time
ENV S6_CMD_WAIT_FOR_SERVICES=1

# Copy root filesystem
COPY rootfs /

# Install Dispatcharr
ARG DISPATCHARR_VERSION="latest"
RUN \
    # Create directories
    mkdir -p /app \
    \
    # Download and install Dispatcharr
    && case "${TARGETPLATFORM}" in \
        "linux/amd64") export ARCH="amd64" ;; \
        "linux/arm64") export ARCH="arm64" ;; \
        "linux/arm/v7") export ARCH="armv7" ;; \
        *) echo "Unsupported platform ${TARGETPLATFORM}" && exit 1 ;; \
    esac \
    \
    # Get latest release or use specified version
    && if [ "${DISPATCHARR_VERSION}" = "latest" ]; then \
        DISPATCHARR_VERSION=$(curl -s "https://api.github.com/repos/dispatcharr/dispatcharr/releases/latest" | jq -r '.tag_name'); \
    fi \
    \
    # Download and extract
    && curl -L "https://github.com/dispatcharr/dispatcharr/releases/download/${DISPATCHARR_VERSION}/dispatcharr_${DISPATCHARR_VERSION}_Linux_${ARCH}.tar.gz" \
        | tar xz -C /app \
    \
    # Make executable
    && chmod +x /app/dispatcharr

# Volumes
VOLUME [ "/data" ]

# Labels
LABEL \
    io.hass.name="Dispatcharr" \
    io.hass.description="IPTV & stream management with Home Assistant integration" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version="${BUILD_VERSION}" \
    maintainer="Stainy3244" \
    org.opencontainers.image.title="Dispatcharr" \
    org.opencontainers.image.description="IPTV & stream management with Home Assistant integration" \
    org.opencontainers.image.source="https://github.com/Stainy3244/dispatcharr" \
    org.opencontainers.image.licenses="MIT"
