FROM jenkins/jenkins:lts

USER root

# Install Docker CLI, Node.js v20, and Docker Compose plugin manually
RUN apt-get update && \
    apt-get install -y \
        curl \
        gnupg2 \
        ca-certificates \
        apt-transport-https \
        docker.io && \
    curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get update && \
    apt-get install -y nodejs && \
    npm install -g npm@latest && \
    # Install Docker Compose plugin manually
    mkdir -p /usr/local/lib/docker/cli-plugins && \
    curl -SL https://github.com/docker/compose/releases/download/v2.24.6/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose && \
    chmod +x /usr/local/lib/docker/cli-plugins/docker-compose && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Confirm installations
RUN node -v && npm -v && docker --version && docker compose version
