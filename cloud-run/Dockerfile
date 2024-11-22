FROM debian:buster-slim

ENV VAULT_VERSION=1.18.1
ENV PORT=8200
ENV VAULT_ADDR=http://127.0.0.1:8200

# Install dependencies
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && \
    apt-get install --no-install-recommends -y \
    ca-certificates \
    wget \
    unzip && \
    rm -rf /var/lib/apt/lists/*

# Download and install Vault
RUN wget -q https://releases.hashicorp.com/vault/${VAULT_VERSION}/vault_${VAULT_VERSION}_linux_amd64.zip && \
    unzip vault_${VAULT_VERSION}_linux_amd64.zip -d /usr/local/bin && \
    rm vault_${VAULT_VERSION}_linux_amd64.zip

# Create necessary directories
RUN mkdir -p /etc/vault /vault/data

# Copy config
COPY config.hcl /etc/vault/config.hcl

# Use non-root user
RUN useradd -r -u 1000 -g root vault
RUN chown -R vault:root /etc/vault /vault
USER vault

# Expose port
EXPOSE ${PORT}

CMD ["/usr/local/bin/vault", "server", "-config=/etc/vault/config.hcl"]