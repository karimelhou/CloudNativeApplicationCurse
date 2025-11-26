# === STAGE 1 : Build / preparation ===
FROM node:18 AS builder

WORKDIR /app

# Install all deps (dev + prod)
COPY package*.json ./
RUN npm install

# Copy the whole backend source (includes src/, prisma/, seed/, etc.)
COPY . .

# Generate Prisma client
RUN npx prisma generate

# === STAGE 2 : Production image ===
FROM node:18

WORKDIR /app

ENV NODE_ENV=production

# Create non-root user
RUN useradd -m appuser

# Copy only what we need to run the app
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/prisma ./prisma
COPY --from=builder /app/src ./src
COPY --from=builder /app/seed ./seed

EXPOSE 3000

USER appuser

CMD ["npm", "start"]
