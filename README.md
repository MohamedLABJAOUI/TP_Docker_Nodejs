# TP Docker-Node.js 
Ce projet est un TP pour apprendre à utiliser ""Docker" avec "Node.js" et ""PostgreSQL"

# Objectif du TP : 
        L’objectif de ce TP est de vous permettre de mettre en place une application Node.js avec 
        une base de données PostgreSQL, le tout conteneurisé avec Docker et orchestré avec Docker 
        Compose. Vous allez construire une API REST simple qui communique avec une base de 
        données PostgreSQL et est déployée sur plusieurs containers Docker. 
        
### Partie 1 : Mise en place de l’application Node.js 
1. Créez un projet Node.js dans un dossier `backend`.  - Initialisez-le avec `npm init` et installez **Express.js** ainsi que le client **pg** pour 
PostgreSQL.          
3. Créez une API avec une route `/users` qui : - Se connecte à une base de données PostgreSQL. - Récupère une liste d’utilisateurs dans une table `users` (id, nom) et la retourne en JSON. 
4. Vous devez utiliser les variables d’environnement pour la configuration de la base de 
données (nom de la base, utilisateur, mot de passe). 
### Partie 2 : Dockerisation du backend 
1. Créez un fichier `Dockerfile` dans le dossier `backend` : - Basez-vous sur l’image officielle de Node.js. - Copiez les fichiers du projet et installez les dépendances. - Exposez le port 3000 de votre container pour que l’application soit accessible. 
2. Créez un fichier `.dockerignore` pour éviter d'inclure les fichiers inutiles dans l’image 
Docker (comme `node_modules`). 
3. Construisez et testez votre image Docker en local avec les commandes `docker build` et 
`docker run`. 
### Partie 3 : Dockerisation de la base de données PostgreSQL 
1. Créez un fichier `docker-compose.yml` à la racine de votre projet : - Ajoutez un service pour PostgreSQL, en utilisant l’image officielle `postgres`. - Assurez-vous que le service PostgreSQL expose le port `5432` et que ses données sont 
persistées dans un volume Docker. - Définissez les variables d’environnement nécessaires pour la base de données (nom de la 
base, utilisateur, mot de passe). 
### Partie 4 : Orchestration avec Docker Compose 
1. Dans le fichier `docker-compose.yml`, ajoutez un service pour votre backend Node.js : - Ce service doit dépendre du service PostgreSQL. - Vous devez également exposer le port 3000 du backend pour pouvoir tester l’API. 
2. Reliez les services (backend et PostgreSQL) en utilisant le réseau Docker interne, ce qui 
permettra à votre application de se connecter à PostgreSQL avec le nom du service comme 
adresse d'hôte (ex : `db`). 
3. Ajoutez un fichier `.env` pour définir les variables d’environnement utilisées dans les 
services (comme les identifiants de la base de données). 
### Partie 5 : Initialisation de la base de données 
1. Initialisez la base de données en ajoutant une table `users` et quelques données de test 
dans PostgreSQL.  - Vous pouvez utiliser un fichier `init.sql` qui sera exécuté lors du démarrage du service 
PostgreSQL (via Docker Compose). 
### Partie 6 : Lancer l’application et tester 
1. Lancez l’ensemble de l’application avec Docker Compose : `docker-compose up --build`. 
2. Vérifiez que les services démarrent correctement et que l’application est accessible sur 
`http://localhost:3000`. 
3. Testez l’API `/users` en y accédant via votre navigateur ou un outil comme Postman pour 
vérifier qu’elle retourne bien les utilisateurs de la base de données.


# Reponse :
1️⃣ Créer un projet Node.js dans un dossier "backend"

          mkdir backend && cd backend
          npm init -y
          
2️⃣ Installer Express.js et le client PostgreSQL (pg):

          npm install express pg dotenv
          
3️⃣ Créer une API avec une route /users qui :
      /Se connecte à PostgreSQL
      /Récupère les utilisateurs dans une table users
      /Retourne les résultats en JSON

4️⃣ Utiliser des variables d’environnement pour la configuration de la base de données (.env).

🐳Partie 2 : Dockerisation du backend
1️⃣ Créer un fichier Dockerfile dans backend

             FROM node:18    # Utiliser l'image Node.js officielle
             WORKDIR /app
             
             COPY package.json ./ # Copier les fichiers et installer les dépendances
             RUN npm install
             COPY . .

            EXPOSE 3000 # Exposer le port 3000 et démarrer l’application
            CMD ["node", "index.js"]
2️⃣ Créer un fichier .dockerignore

            node_modules
            npm-debug.log
            .env
            
3️⃣ Construire et tester l’image Docker
 
            docker build -t node-app .
            docker run -p 3000:3000 node-app
            
🐘 Partie 3 : Dockerisation de PostgreSQL
1️⃣ Créer un fichier docker-compose.yml à la racine du projet

             version: '3.8'
             services:
                      db:
                          image: postgres
                          restart: always
             environment:
                          POSTGRES_USER: user
                          POSTGRES_PASSWORD: password
                          POSTGRES_DB: mydatabase
                   ports:
                          - "5432:5432"
                 volumes:
                          - pgdata:/var/lib/postgresql/data

                 volumes:
                           pgdata:
⚙️ Partie 4 : Orchestration avec Docker Compose
1️⃣ Ajouter un service backend dans docker-compose.yml

                 backend:
                         build: ./backend
                   ports:
                         - "3000:3000"
              depends_on:
                         - db
             environment:
                          DB_HOST: db
                          DB_USER: user
                          DB_PASSWORD: password
                          DB_NAME: mydatabase
                          
2️⃣ Ajouter un fichier .env pour stocker les variables d’environnement
              DB_HOST=db
              DB_USER=user
              DB_PASSWORD=password
              DB_NAME=mydatabase
🗄️ Partie 5 : Initialisation de la base de données
1️⃣ Créer un fichier init.sql

               CREATE TABLE users (
                                    id SERIAL PRIMARY KEY,
                                    nom VARCHAR(50) NOT NULL
                                  );

               INSERT INTO users (nom) VALUES ('Examplename');
2️⃣ Modifier docker-compose.yml pour exécuter le script au démarrage

                volumes:
      -                 ./init.sql:/docker-entrypoint-initdb.d/init.sql
🚀 Partie 6 : Lancer l’application et tester
1️⃣ Démarrer l’application avec Docker Compose

          docker-compose up --build
2️⃣ Vérifier que l’API est accessible

          URL : http://localhost:3000/users
# powershell cmd :

PS C:\TP_Docker_Nodejs> cd backend
PS C:\TP_Docker_Nodejs\backend> npm init -y
Wrote to C:\TP_Docker_Nodejs\backend\package.json:

{
  "name": "backend",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": ""
}



PS C:\TP_Docker_Nodejs\backend> npm install express pg dotenv

added 81 packages, and audited 82 packages in 8s

15 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
PS C:\TP_Docker_Nodejs\backend> ni index.js


    Directory: C:\Users\user\TP_Docker_Nodejs\backend


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          4/4/2025   2:30 PM              0 index.js


PS C:\TP_Docker_Nodejs\backend> ni .env


    Directory: C:\TP_Docker_Nodejs\backend


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          4/4/2025   2:31 PM              0 .env


PS C:\TP_Docker_Nodejs\backend> ni Dockerfile


    Directory: C:\TP_Docker_Nodejs\backend


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          4/4/2025   2:32 PM              0 Dockerfile


PS C:\TP_Docker_Nodejs\backend> ni .dockerignore


    Directory: C:\TP_Docker_Nodejs\backend


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          4/4/2025   2:32 PM              0 .dockerignore


PS C:\TP_Docker_Nodejs\backend> docker build -t nodejs-api .
ERROR: error during connect: Head "http://%2F%2F.%2Fpipe%2FdockerDesktopLinuxEngine/_ping": open //./pipe/dockerDesktopLinuxEngine: The system cannot find the file specified.
PS C:\TP_Docker_Nodejs\backend> docker info
Client:
 Version:    27.3.1
 Context:    desktop-linux
 Debug Mode: false
 Plugins:
  ai: Ask Gordon - Docker Agent (Docker Inc.)
    Version:  v0.1.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-ai.exe
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.18.0-desktop.2
    Path:     C:\Program Files\Docker\cli-plugins\docker-buildx.exe
  compose: Docker Compose (Docker Inc.)
    Version:  v2.30.3-desktop.1
    Path:     C:\Program Files\Docker\cli-plugins\docker-compose.exe
  debug: Get a shell into any image or container (Docker Inc.)
    Version:  0.0.37
    Path:     C:\Program Files\Docker\cli-plugins\docker-debug.exe
  desktop: Docker Desktop commands (Alpha) (Docker Inc.)
    Version:  v0.0.15
    Path:     C:\Program Files\Docker\cli-plugins\docker-desktop.exe
  dev: Docker Dev Environments (Docker Inc.)
    Version:  v0.1.2
    Path:     C:\Program Files\Docker\cli-plugins\docker-dev.exe
  extension: Manages Docker extensions (Docker Inc.)
    Version:  v0.2.27
    Path:     C:\Program Files\Docker\cli-plugins\docker-extension.exe
  feedback: Provide feedback, right in your terminal! (Docker Inc.)
    Version:  v1.0.5
    Path:     C:\Program Files\Docker\cli-plugins\docker-feedback.exe
  init: Creates Docker-related starter files for your project (Docker Inc.)
    Version:  v1.4.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-init.exe
  sbom: View the packaged-based Software Bill Of Materials (SBOM) for an image (Anchore Inc.)
    Version:  0.6.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-sbom.exe
  scout: Docker Scout (Docker Inc.)
    Version:  v1.15.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-scout.exe

Server:
 Containers: 4
  Running: 0
  Paused: 0
  Stopped: 4
 Images: 4
 Server Version: 27.3.1
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 Swarm: inactive
 Runtimes: nvidia runc io.containerd.runc.v2
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 472731909fa34bd7bc9c087e4c27943f9835f111
 runc version: v1.1.13-0-g58aa920
 init version: de40ad0
 Security Options:
  seccomp
   Profile: unconfined
 Kernel Version: 5.15.167.4-microsoft-standard-WSL2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 8
 Total Memory: 3.732GiB
 Name: docker-desktop
 ID: 5e8aa5ce-11bf-48c8-b8cd-90fea355cc2c
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 HTTP Proxy: http.docker.internal:3128
 HTTPS Proxy: http.docker.internal:3128
 No Proxy: hubproxy.docker.internal
 Labels:
  com.docker.desktop.address=npipe://\\.\pipe\docker_cli
 Experimental: false
 Insecure Registries:
  hubproxy.docker.internal:5555
  127.0.0.0/8
 Live Restore Enabled: false

WARNING: No blkio throttle.read_bps_device support
WARNING: No blkio throttle.write_bps_device support
WARNING: No blkio throttle.read_iops_device support
WARNING: No blkio throttle.write_iops_device support
WARNING: daemon is not using the default seccomp profile
PS C:\Users\user\TP_Docker_Nodejs\backend> docker build -t nodejs-api .
[+] Building 1235.6s (11/11) FINISHED            docker:default
 => [internal] load build definition from Dockerfile       0.1s
 => => transferring dockerfile: 401B                       0.0s
 => [internal] load metadata for docker.io/library/node:1  2.7s
 => [internal] load .dockerignore                          0.1s
 => => transferring context: 60B                           0.0s
 => [1/6] FROM docker.io/library/node:18@sha256:3c5624  1217.8s
 => => resolve docker.io/library/node:18@sha256:3c5624851  0.0s
 => => sha256:2e36833b68b10fbe7cb863e3bc418f9 447B / 447B  0.2s
 => => sha256:7202769db681ba7a16091aca6a0 1.25MB / 1.25MB  2.9s
 => => sha256:5aa9b40886c859f533da114 45.68MB / 45.68MB  749.0s
 => => sha256:d28ef26f74e7b12fddcf44897b6 3.33kB / 3.33kB  1.4s
 => => sha256:353e14e5cc47664fba71 211.35MB / 211.35MB  1197.2s
 => => sha256:255774e0027b72d2327719e 64.40MB / 64.40MB  865.0s
 => => sha256:091eb8249475f42de217265 24.01MB / 24.01MB  173.8s
 => => sha256:7cd785773db44407e20a679 48.47MB / 48.47MB  684.6s
 => => extracting sha256:7cd785773db44407e20a679ce5439222  1.8s
 => => extracting sha256:091eb8249475f42de217265c501e0186  1.0s
 => => extracting sha256:255774e0027b72d2327719e78dbad5ad  2.6s
 => => extracting sha256:353e14e5cc47664fba714a7da288001  15.5s
 => => extracting sha256:d28ef26f74e7b12fddcf44897b6afca5  0.0s
 => => extracting sha256:5aa9b40886c859f533da11496ce0fe8f  4.4s
 => => extracting sha256:7202769db681ba7a16091aca6a0741f0  0.1s
 => => extracting sha256:2e36833b68b10fbe7cb863e3bc418f9f  0.0s
 => [internal] load build context                          0.1s
 => => transferring context: 36.10kB                       0.0s
 => [2/6] WORKDIR /app                                     0.6s
 => [3/6] COPY package.json ./                             0.1s
 => [4/6] COPY package-lock.json ./                        0.1s
 => [5/6] RUN npm install                                 12.2s
 => [6/6] COPY . .                                         0.1s
 => exporting to image                                     1.2s
 => => exporting layers                                    0.5s
 => => exporting manifest sha256:2a2b3c01eb56c5bce91d3c23  0.0s
 => => exporting config sha256:a4781d97e0b8906a3f2c011939  0.0s
 => => exporting attestation manifest sha256:598f0b1a6c4d  0.0s
 => => exporting manifest list sha256:35002f80e3a014652eb  0.0s
 => => naming to docker.io/library/nodejs-api:latest       0.0s
 => => unpacking to docker.io/library/nodejs-api:latest    0.4s
PS C:\Users\user\TP_Docker_Nodejs\backend> docker run -p 3000:3000 nodejs-api
🚀 Serveur démarré sur http://localhost:3000

PS C:\TP_Docker_Nodejs> ni docker-compose.yml


    Directory: C:\TP_Docker_Nodejs


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          4/4/2025   3:01 PM              0 docker-compose.yml


PS C:\TP_Docker_Nodejs> ni init.sql


    Directory: C:\TP_Docker_Nodejs


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          4/4/2025   3:02 PM              0 init.sql


PS C:\TP_Docker_Nodejs> docker-compose up --build
time="2025-04-04T15:03:01Z" level=warning msg="C:\\Users\\user\\TP_Docker_Nodejs\\docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion"
[+] Running 14/15
 - db [⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿] 153.2MB / 156.2MB Pulling                                                                5515.5s
   ✔ 140970538145 Download complete                                                                                0.8s
   ✔ e02d97322fc6 Download complete                                                                               13.9s
   ✔ 133acbc970df Download complete                                                                               42.1s
   ✔ 221788d72606 Download complete                                                                                4.4s
   ✔ 6e909acdb790 Download complete                                                                              120.2s
   ✔ 9bcedd9434e7 Download complete                                                                                8.9s
   ✔ e7a2d9e24ab0 Download complete                                                                                4.3s
   ✔ fec99121872b Download complete                                                                                4.3s
   ✔ 1824bd6b75d7 Download complete                                                                                4.3s
   ✔ fbad2bf2d5e6 Download complete                                                                              184.8s
   ✔ db9643c6baf3 Download complete                                                                               42.6s
   ✔ fc8982ec96d9 Download complete                                                                                4.4s
   ✔ a96cb29b0d13 Download complete                                                                                4.2s
   ✔ e5f43b682bc0 Download complete                                                                                4.4s
