# Stage 1: Development
FROM node:22-alpine AS development
WORKDIR /frontend_app

COPY package*.json ./
RUN if [ -f package-lock.json ]; then npm ci; else npm install; fi

COPY . .
EXPOSE 5173
CMD ["npm", "run", "dev", "--", "--host"]

# Stage 2: Build
FROM development AS builder
RUN npm run build

# Stage 3: Production
FROM nginxinc/nginx-unprivileged:alpine AS production
COPY --from=builder /frontend_app/dist /usr/share/nginx/html

COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 8080
CMD ["nginx", "-g", "daemon off;"]

