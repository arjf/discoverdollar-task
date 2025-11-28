FROM node:18-alpine AS base
WORKDIR /app

FROM base AS deps
COPY package*.json ./
RUN npm ci --only=production

FROM base AS release

# Create non-root user
RUN addgroup -g 1001 -S nodejs && \
  adduser -S nodejs -u 1001

COPY --from=deps /app/node_modules /app/node_modules
COPY . /app/

USER nodejs
EXPOSE 8080
CMD ["node", "/app/server.js"]
