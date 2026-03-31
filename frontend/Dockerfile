# Этап сборки
FROM node:22-alpine AS builder

WORKDIR /app

# Копируем package.json и package-lock.json (обратите внимание на правильные имена)
COPY package*.json ./

# Устанавливаем зависимости
RUN npm ci

# Копируем ВСЕ исходные файлы из ТЕКУЩЕЙ директории
COPY . .

# Собираем приложение
RUN npm run build

# Этап production с Nginx
FROM nginx:alpine

# Копируем собранные файлы в Nginx
COPY --from=builder /app/build /usr/share/nginx/html

# Копируем конфигурацию Nginx
COPY nginx.conf /etc/nginx/nginx.conf

# Создаем непривилегированного пользователя
RUN adduser -D -u 1001 nginxuser && \
    chown -R nginxuser /usr/share/nginx/html && \
    chown -R nginxuser /var/cache/nginx && \
    chown -R nginxuser /var/run && \
    touch /var/run/nginx.pid && \
    chown -R nginxuser /var/run/nginx.pid

USER nginxuser

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]