FROM jenkins/jenkins:lts

USER root

# Install Docker CLI, Node.js v20, and Docker Compose plugin manually
RUN apt-get update && \
    apt-get install -y \
        curl \
        gnupg2 \
        ca-certificates \
        apt-transport-https \
        lsb-release \
        software-properties-common \
        docker.io && \
    curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g npm@latest && \
    # Install Docker Compose plugin manually
    mkdir -p ~/.docker/cli-plugins/ && \
    curl -SL https://github.com/docker/compose/releases/download/v2.24.6/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose && \
    chmod +x ~/.docker/cli-plugins/docker-compose && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Optional: check versions
RUN node -v && npm -v && docker --version && docker compose version

