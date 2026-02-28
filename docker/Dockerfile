FROM node:lts-slim

ARG DEV_UID
ARG DEV_GID

RUN groupmod --gid $DEV_GID node && \
    usermod --uid $DEV_UID --gid $DEV_GID node

RUN npm install -g pnpm

USER node:node

RUN pnpm config set store-dir /home/node/.local/share/pnpm/store
