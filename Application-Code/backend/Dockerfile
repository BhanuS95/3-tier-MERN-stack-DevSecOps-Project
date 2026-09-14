# Stage 1: Build
FROM node:22-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build


# Stage 2: Production
FROM node:22-alpine

WORKDIR /app

RUN groupadd --system --gid appgroup && useradd --system --uid appuser -gid appgroup appuser

COPY package*.json ./
RUN npm install --omit=dev && npm cache clean --force
COPY --from=builder /app/dist ./dist

RUN chown -R appuser:appgroup /app
USER appuser

EXPOSE 3500
CMD ["node", "start"]