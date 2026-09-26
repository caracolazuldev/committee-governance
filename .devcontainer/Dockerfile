FROM node:22-bookworm-slim

RUN apt-get update \
    && apt-get install -y --no-install-recommends python3 python3-venv ca-certificates curl ripgrep \
    && rm -rf /var/lib/apt/lists/*

RUN python3 -m venv /opt/google-tools
ENV PATH="/opt/google-tools/bin:${PATH}"

COPY requirements.txt /tmp/requirements.txt
RUN pip install --no-cache-dir -r /tmp/requirements.txt \
    && npm install --global zapier-platform-cli@19.1.0 \
    && npm cache clean --force

RUN mkdir -p /home/node/.config && chown node:node /home/node/.config

RUN printf '\numask 077\nif [ -r "$HOME/.config/cmte-gvrnce/tokens.env" ]; then\n  set -a\n  . "$HOME/.config/cmte-gvrnce/tokens.env"\n  set +a\nfi\n' >> /home/node/.bashrc \
    && chown node:node /home/node/.bashrc

USER node
WORKDIR /workspaces/cmte-gvrnce