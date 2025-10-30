# Simple React build and serve
FROM node:18-alpine

# Allow setting the backend base URL at build time so Vite has correct import.meta.env
ARG VITE_API_BASE_URL

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install --production=false

# Copy source code
COPY . .

# If a build arg was provided, write a Vite .env so import.meta.env picks it up at build time
RUN if [ -n "$VITE_API_BASE_URL" ]; then echo "VITE_API_BASE_URL=$VITE_API_BASE_URL" > .env; fi

# Build the application
RUN npm run build

# Expose port used by vite preview
EXPOSE 3000

# Serve the built application using vite preview
CMD ["npx", "vite", "preview", "--port", "3000", "--host", "0.0.0.0"]