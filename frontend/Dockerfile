# frontend/Dockerfile
# at top of frontend/Dockerfile build stage
ARG VITE_API_URL
ENV VITE_API_URL=$VITE_API_URL

# then during build, your app's code should read import.meta.env.VITE_API_URL
# Build command remains: RUN npm run build

# Multi-stage build: build with node, serve with nginx
FROM node:18-alpine AS build

WORKDIR /app

# copy package manifest first to leverage cache
COPY package*.json ./
# If using pnpm or yarn, modify accordingly
RUN npm ci --silent

# copy source
COPY . .

# ensure VITE_API_URL or other envs are handled at build time if needed
# build (Vite project produces `dist` folder)
RUN npm run build

# production image
FROM nginx:stable-alpine

# Remove default nginx static files
RUN rm -rf /usr/share/nginx/html/*

# Copy built files from builder
COPY --from=build /app/dist /usr/share/nginx/html

# Copy custom nginx config (for SPA history fallback)
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

# run nginx in foreground
CMD ["nginx", "-g", "daemon off;"]
