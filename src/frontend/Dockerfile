FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./

# modules
RUN npm ci
COPY . .

# build
RUN npm run build -- --configuration=production

# prod image
FROM nginx:alpine AS production
RUN rm -rf /usr/share/nginx/html/*

# nginx conf
COPY --from=build /app/dist/angular-15-crud /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

# non-root user
RUN chown -R nginx:nginx /usr/share/nginx/html && \
  chown -R nginx:nginx /var/cache/nginx && \
  chown -R nginx:nginx /var/log/nginx && \
  touch /var/run/nginx.pid && \
  chown -R nginx:nginx /var/run/nginx.pid

EXPOSE 80
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:80/ || exit 1
CMD ["nginx", "-g", "daemon off;"]
