# === STAGE 1 : Build Vue.js ===
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

# Construire le frontend Vue
RUN npm run build

# === STAGE 2 : Nginx image ===
FROM nginx:alpine

# Supprime la conf par défaut
RUN rm /etc/nginx/conf.d/default.conf

# Copier ta configuration personnalisée
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copier les fichiers buildés
COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
