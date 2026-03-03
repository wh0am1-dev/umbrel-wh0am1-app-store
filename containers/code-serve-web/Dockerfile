FROM debian:bookworm-slim

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    wget \
    gpg \
    && rm -rf /var/lib/apt/lists/*

# Add Microsoft GPG key and VS Code repository
RUN mkdir -p /etc/apt/keyrings \
    && wget -qO- https://packages.microsoft.com/keys/microsoft.asc \
      | gpg --dearmor > /etc/apt/keyrings/packages.microsoft.gpg \
    && chmod 644 /etc/apt/keyrings/packages.microsoft.gpg \
    && echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" \
      > /etc/apt/sources.list.d/vscode.list

# Install VS Code
RUN apt-get update && apt-get install -y --no-install-recommends \
    code \
    && rm -rf /var/lib/apt/lists/*

# Create persistent data folder
RUN mkdir -p /data

# Optional: expose default port used by code serve-web
EXPOSE 8000

# Auto-update on restart:
# Re-run apt update/upgrade for code each container start, then launch serve-web.
CMD ["/bin/sh", "-c", "apt-get update && apt-get install -y --only-upgrade code && exec code serve-web --host 0.0.0.0 --without-connection-token --server-data-dir /data --accept-server-license-terms"]
