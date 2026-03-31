# ------------------------------------------------------------------------------
# 🏗️ Stage 1: Build Stage (Install Dependencies & Prepare App)
# ------------------------------------------------------------------------------

FROM node:21 AS backend-builder

# Set working directory
WORKDIR /app

# Copy package files (better caching)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy remaining source code
COPY . .

# ------------------------------------------------------------------------------
# 🚀 Stage 2: Production Stage (Lightweight & Secure)
# ------------------------------------------------------------------------------

FROM node:21-alpine

# Set working directory
WORKDIR /app

# Set environment to production
ENV NODE_ENV=production

# this line add kyu kar raha he because i am used node:21-alpine base image
# by default alpine me wget nehi hota he isliye job me build karta hu backend unhealthy show kar raha he
# so that i used this line ..automatic download the wget command
# RUN apk add --no-cache wget

# Create non-root user for security
RUN addgroup --system appgroup \
    && adduser -S appuser -G appgroup

# Switch to non-root user
USER appuser

# Copy only required files from builder stage
COPY --from=backend-builder --chown=appuser:appgroup /app/package*.json ./
COPY --from=backend-builder --chown=appuser:appgroup /app/node_modules ./node_modules
COPY --from=backend-builder --chown=appuser:appgroup /app .

# Expose application port
EXPOSE 5000

HEALTHCHECK --interval=30s --timeout=10s --start-period=30s --retries=3 \
  CMD wget -q --spider http://localhost:5000/health  || exit 1

# Start the application
CMD ["node", "server.js"]
