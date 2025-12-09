# Stage 1: Build Quasar SPA
FROM node:20-alpine AS builder
WORKDIR /app

# Copy project files
COPY . .

# Install dependencies
RUN npm ci --legacy-peer-deps

# Build Quasar SPA
ARG VITE_API_URL
ENV VITE_API_URL=$VITE_API_URL
RUN npx quasar build -m spa

# Stage 2: Production (Nginx serve static files)
FROM nginx:1.27-alpine

# Copy build artifacts from stage 1
COPY --from=builder /app/dist/spa /usr/share/nginx/html

# Expose port
EXPOSE 80

# Run Nginx
CMD ["nginx", "-g", "daemon off;"]