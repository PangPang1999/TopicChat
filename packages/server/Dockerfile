# syntax=docker/dockerfile:1
FROM oven/bun:1.2.21 AS runner

WORKDIR /app

# Copy workspace manifests first for better layer caching
COPY bun.lock package.json ./
COPY packages/server/package.json packages/server/
COPY packages/client/package.json packages/client/

RUN bun install

# Copy source code
COPY packages/server ./packages/server
COPY tsconfig.json ./tsconfig.json

WORKDIR /app/packages/server

ENV NODE_ENV=production
ENV PORT=3000

EXPOSE 3000

CMD ["bun", "run", "start"]
