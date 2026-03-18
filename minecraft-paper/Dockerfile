ARG BUILD_FROM=ghcr.io/hassio-addons/base:16.3.3
FROM $BUILD_FROM

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ARG BUILD_ARCH

# Install Java 21, curl, jq and download rcon-cli mapping arm64/amd64 correctly
RUN \
    echo "Installing dependencies..." \
    && apk add --no-cache \
        openjdk21-jre-headless \
        curl \
        jq \
    \
    && if [ "${BUILD_ARCH}" = "aarch64" ]; then \
         RCON_ARCH="arm64"; \
       elif [ "${BUILD_ARCH}" = "amd64" ]; then \
         RCON_ARCH="amd64"; \
       else \
         RCON_ARCH="amd64"; \
       fi \
    && curl -sL "https://github.com/itzg/rcon-cli/releases/download/1.6.9/rcon-cli_1.6.9_linux_${RCON_ARCH}.tar.gz" | tar xz -C /usr/local/bin rcon-cli \
    && chmod +x /usr/local/bin/rcon-cli

# Copy root filesystem (s6-overlay structure)
COPY rootfs /

# Set the working directory where our persistent data will mount
WORKDIR /data
