# Utiliser l'image officielle Node.js
FROM node:18

# Définir le répertoire de travail
WORKDIR /app

# Copier les fichiers du projet
COPY package.json ./
COPY package-lock.json ./
RUN npm install

# Copier le reste du projet
COPY . .

# Exposer le port de l’API
EXPOSE 3000

# Commande pour lancer l’application
CMD ["node", "index.js"]
