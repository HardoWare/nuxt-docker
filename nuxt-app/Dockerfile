LABEL authors="HardWare"

# Versions
FROM node:20-alpine AS node_upstream

# Base stage for dev and build
FROM node_upstream AS base

RUN apk add --no-cache libc6-compat curl

WORKDIR /app

RUN corepack enable && \
    corepack prepare --activate pnpm@latest && \
    pnpm config -g set store-dir /.pnpm-store && \
    ls


# Development image
FROM base AS dev

WORKDIR /app

COPY --link package.json ./

COPY --link . .

EXPOSE 3000
ENV NODE_ENV development
ENV PORT 3000
ENV HOSTNAME localhost

# Sometimes occure issues with nitro .sock, if so switch CMD
#CMD ["sh", "-c", "pnpm install; rm /tmp/nitro -rf;pnpm nuxi dev"]

CMD ["sh", "-c", "pnpm install; pnpm nuxi dev"]


# Build image
FROM base AS builder

COPY --link package.json pnpm-lock.yaml ./
RUN pnpm fetch --prod

COPY --link . .

RUN pnpm install --frozen-lockfile --offline --prod && \
    pnpm run build

# Production image
FROM node_upstream AS prod

WORKDIR /app

ENV NODE_ENV production

RUN addgroup --system --gid 1001 nodejs; \
    adduser --system --uid 1001 nuxtjs

# Copy only necessary files for production
COPY --from=builder /app/public ./public
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/.output ./.output

# Set the correct permissions for the .output directory
#RUN mkdir -p .nuxt && \
#    chown -R nuxtjs:nodejs .nuxt .output
RUN rm -rf .nuxt && \
    mkdir -p .nuxt && \
    chown -R nuxtjs:nodejs .nuxt .output

USER nuxtjs

EXPOSE 3000

ENV PORT 3000
ENV HOSTNAME "0.0.0.0"

CMD ["node", ".output/server/index.mjs"]