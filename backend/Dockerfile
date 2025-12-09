# Stage 1: Build
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .

# Stage 2: Production
FROM node:20-alpine

WORKDIR /app

# Copy built artifacts and dependencies from builder
COPY --from=builder /app /app

# Create logs directory and set ownership to node user
RUN mkdir -p logs && chown -R node:node /app

# Switch to non-root user
USER node

# Healthcheck
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD wget --quiet --tries=1 --spider http://localhost:3000/api/demo || exit 1

EXPOSE 3000

CMD ["node", "server.js"]
