# ----------------- Stage 1: Build Stage -----------------
FROM node:21

# Set working directory inside the container
WORKDIR /app

# Copy package files first (for caching)
COPY package*.json ./

# Disable prepare script safely (npm v10+ compatible)
RUN npm pkg delete scripts.prepare && npm cache clean --force

# Install dependencies
RUN npm install --legacy-peer-deps

# Copy environment file (optional)
COPY .env.sample .env.local

# Copy all source files
COPY . .

# Expose frontend port (React/Vite/Next defaults to 3000)
EXPOSE 5173

# Start the frontend server (with host binding for Docker)
CMD ["npm", "run", "dev", "--", "--host"]
