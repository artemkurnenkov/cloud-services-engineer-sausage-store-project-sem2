# -- Образ для сборки
FROM node:8 AS build

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci

COPY . .
RUN npm run build

# -- Runtime образ
FROM nginx:1.30.4-alpine3.24

COPY --from=build /app/dist/frontend/ /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
