# backend/Dockerfile
FROM node:18-alpine

# Create app directory
WORKDIR /usr/src/app

# copy package files first for layer caching
COPY package*.json ./

# install deps (npm ci for reproducible installs)
RUN npm ci --production --silent

# copy source
COPY . .

# if you need build step (e.g. TypeScript), run it here:
# RUN npm run build

# expose the port the backend listens on; adjust if your app uses a different port
ENV PORT=5000
EXPOSE 5000

# default command - adjust if your start script is different (e.g. npm run start)
CMD ["node", "server.js"]
