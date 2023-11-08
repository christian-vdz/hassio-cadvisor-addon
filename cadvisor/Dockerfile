ARG BUILD_FROM=ghcr.io/hassio-addons/base:14.3.1
ARG BUILD_VERSION
ARG BUILD_UPSTREAM="v0.47.2"
FROM "gcr.io/cadvisor/cadvisor:${BUILD_UPSTREAM}" as cadvisor
FROM ${BUILD_FROM}
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Copy cadvisor from image
COPY --from=cadvisor /usr/bin/cadvisor /usr/bin/cadvisor

# Setup base
RUN \
    apk add --no-cache \
        ipmitool=1.8.18-r0 \
        libpfm=4.11.0-r0 \
        libipmctl=02.00.00.3723-r0 \
        lm_sensors=3.6.0-r0 \
        wget=1.21.1-r1

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="christian-vdz (https://github.com/christian-vdz)" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Cadvisor Hassio add-on" \
    org.opencontainers.image.authors="christian-vdz (https://github.com/christian-vdz)" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://github.com/christian-vdz" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}

# Healthcheck
ENV CADVISOR_HEALTHCHECK_URL=http://localhost:8080/healthz
HEALTHCHECK CMD wget --quiet --tries=1 --spider $CADVISOR_HEALTHCHECK_URL || exit 1

CMD [ "/run.sh" ]
