FROM ubuntu:26.04

# Environment variables
ENV LANG=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    LC_ALL=en_US.UTF-8 \
    TZ="America/Detroit" \
    DEBIAN_FRONTEND=noninteractive

# Install packages and configure locale
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        wget \
        locales \
        git \
        build-essential \
        openssl \
        zsh && \
    locale-gen en_US.UTF-8 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install Oh My ZSH (uses "robbyrussell" theme, no plugins)
RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.2.1/zsh-in-docker.sh)" -- \
    -t robbyrussell

# Create unprivileged user for downstream images
ARG APP_UID=10001
ARG APP_GID=10001
RUN groupadd -g ${APP_GID} appuser && \
    useradd -m -u ${APP_UID} -g ${APP_GID} -s /bin/zsh appuser

# Default working directory
WORKDIR /apps
RUN chown appuser:appuser /apps

# Note: USER is NOT set here - downstream images should set USER when appropriate
