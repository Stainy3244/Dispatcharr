#################
# 1 Base Image #
#################

# Use official Dispatcharr image from GHCR
FROM ghcr.io/dispatcharr/dispatcharr:latest as buildstage

# Set S6 wait time
ENV S6_CMD_WAIT_FOR_SERVICES=1 \
    S6_CMD_WAIT_FOR_SERVICES_MAXTIME=0 \
    S6_SERVICES_GRACETIME=0

################
# 2 Final Image #
################

FROM buildstage

# Set S6 wait time
ENV S6_CMD_WAIT_FOR_SERVICES=1

# Copy root filesystem for HA add-on
COPY rootfs /

# Ensure /app directory exists and Dispatcharr binary is executable
RUN mkdir -p /app \
    && chmod +x /app/dispatcharr || true

# Volumes
VOLUME [ "/data" ]

# Expose web UI port
EXPOSE 9191

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
    org.opencontainers.image.source="https://github.com/Dispatcharr/Dispatcharr" \
    org.opencontainers.image.licenses="MIT"
