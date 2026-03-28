
# ------------------------------------------------------------------------------
# 🏗️ Stage 1: Frontend Build Stage (Vite / React App)
# ------------------------------------------------------------------------------

FROM node:21 AS frontend-builder

#  Set working directory inside container
WORKDIR /app

#  Copy dependency files (for better caching)
COPY package*.json ./

# Install all dependencies
RUN npm install

#  Copy complete source code to working dir
COPY . .

#  Set environment to production in this stage no dev dependencies are included
ENV NODE_ENV=production

#  Inject Backend API URL (Build-time variable)
ENV VITE_API_PATH=${VITE_API_PATH}

#  Build the frontend app (creates /dist folder)
RUN npm run build

# ------------------------------------------------------------------------------
# ✅ Output: Optimized static files inside /app/dist
# Ready to be served via Nginx or any static server
# ------------------------------------------------------------------------------s
