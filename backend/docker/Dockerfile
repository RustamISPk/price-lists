# Этап сборки
FROM node:22-alpine AS builder

WORKDIR /app

# Копируем файлы package.json и package-lock.json
COPY package*.json ./
COPY tsconfig*.json ./

# Устанавливаем зависимости
RUN npm ci

# Копируем исходный код
COPY .. .

# Собираем приложение
RUN npm run build

# Этап production
FROM node:22-alpine

WORKDIR /app

# Копируем собранное приложение и зависимости
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package*.json ./

# Создаем непривилегированного пользователя
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

USER nodejs

# Открываем порт
EXPOSE 3000

# Запускаем приложение
CMD ["node", "dist/main"]