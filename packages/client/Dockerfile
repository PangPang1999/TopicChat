# syntax=docker/dockerfile:1
FROM oven/bun:1.2.21 AS builder

WORKDIR /app

# Copy manifests for dependency install
COPY bun.lock package.json ./
COPY packages/client/package.json packages/client/
COPY packages/server/package.json packages/server/

RUN bun install

# Copy source files for build
COPY tsconfig.json ./tsconfig.json
COPY packages ./packages

WORKDIR /app/packages/client

RUN bun run build

FROM nginx:1.27-alpine AS runner

COPY packages/client/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/packages/client/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
