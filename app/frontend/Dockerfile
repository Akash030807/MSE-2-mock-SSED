
# Stage 1: Build React application
FROM node:22-alpine AS builder

WORKDIR /app

# Copy package configuration first for better caching
COPY package*.json ./

# Install dependencies required for building
RUN npm install

# Copy application source
COPY . .

# Build production frontend
RUN npm run build


# Stage 2: Serve using unprivileged Nginx
FROM nginxinc/nginx-unprivileged:1.27-alpine

# Copy generated static files
COPY --from=builder /app/dist /usr/share/nginx/html

# Nginx unprivileged image listens on port 8080
EXPOSE 8080

# Health check
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
    CMD wget -q -O /dev/null http://127.0.0.1:8080/ || exit 1
