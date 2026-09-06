FROM mcr.microsoft.com/devcontainers/base:ubuntu

# Control Codespace image: OpenHands Agent Canvas + Grok Build + gh.
# No product SDKs. Codespaces using this image expose only port 8000.

ENV DEBIAN_FRONTEND=noninteractive \
    GROK_BIN_DIR=/usr/local/bin \
    npm_config_update_notifier=false \
    PATH="/usr/local/bin:${PATH}"

USER root

RUN apt-get update \
 && apt-get install -y --no-install-recommends ca-certificates curl gnupg tmux openssl \
 && mkdir -p /etc/apt/keyrings \
 && curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg \
      -o /etc/apt/keyrings/githubcli-archive-keyring.gpg \
 && chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
 && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
      > /etc/apt/sources.list.d/github-cli.list \
 && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key \
      | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg \
 && echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_22.x nodistro main" \
      > /etc/apt/sources.list.d/nodesource.list \
 && apt-get update \
 && apt-get install -y --no-install-recommends gh nodejs \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN curl -LsSf https://astral.sh/uv/install.sh | env UV_INSTALL_DIR=/usr/local/bin sh \
 && uv --version && uvx --version

RUN curl -fsSL https://x.ai/cli/install.sh | bash \
 && command -v grok

COPY package.json /opt/bellen/package.json
RUN npm install -g "@openhands/agent-canvas@$(node -p "require('/opt/bellen/package.json').dependencies['@openhands/agent-canvas']")" \
 && npm cache clean --force \
 && agent-canvas --version

USER vscode
WORKDIR /workspaces

# Warm uvx so the first Codespace start does not download PyPI wheels.
RUN node -e " \
  const { execSync } = require('child_process'); \
  const path = require('path'); \
  const fs = require('fs'); \
  const root = path.join(execSync('npm root -g', { encoding: 'utf8' }).trim(), '@openhands/agent-canvas'); \
  const d = JSON.parse(fs.readFileSync(path.join(root, 'config/defaults.json'), 'utf8')); \
  execSync('uvx --from ' + d.packages.agentServer + '==' + d.versions.agentServer + ' agent-server --help', { stdio: 'inherit' }); \
  execSync('uvx --from ' + d.packages.automation + '==' + d.versions.automation + ' uvicorn --help', { stdio: 'inherit' }); \
"

USER vscode
