# Stage 1: Build
FROM node:21-alpine AS builder
# Set working directory
WORKDIR /app

# Install build dependencies (bash for npm post-install scripts, if needed)
RUN apk add --no-cache bash

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application source code
COPY . .

# Build the Vite application
RUN npm run build

# Stage 2: Serve
FROM nginx:alpine AS production

# Copy build output to nginx's html directory
COPY --from=builder /app/build /usr/share/nginx/html

# Copy custom nginx configuration if needed
COPY Nginx/nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]