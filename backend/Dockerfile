# backend/Dockerfile
FROM node:18-alpine AS builder
WORKDIR /app

# copy package jsons and install
COPY package*.json ./
RUN npm ci --silent

# copy rest
COPY . .

# if you have a build script (optional)
# RUN npm run build

FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app /app
ENV NODE_ENV=production
EXPOSE 5000
CMD ["node", "server.js"]
