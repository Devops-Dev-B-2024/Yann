# TP3
### Fait par Yann Sady

## 2. Créer un Dockerfile qui permet de lancer une application NodeJS (v18-alpine ou v20)

J'ai crée le Dockerfile :
```
FROM node:18-alpine

WORKDIR /home/node/app

COPY ./e-bakery/ ./

RUN npm install

EXPOSE 8000

CMD ["node", "server.js"]
``` 

## 3. Utilisez docker pour lancer une image de base de données (mysql ou mariadb)

J'ai fait la commande : ```docker run -d -p 3000:80 --name mon_container_mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=yes mysql```

## 4. Rebuildez votre image docker et relancez un container, vérifiez que vous arrivez à utiliser l'app
D'abord j'ai commencé par build mon image : ```docker build -t api-e-bakery .```

Ensuite que l'api puisse avoir accès à la bdd, j'ai crée un network ```docker network create reseau_api```

Puis j'ai ajouté mes 2 containers dans ce network :
- ```docker network connect reseau_api mon_container_mysql```
- ```docker network connect reseau_api container_api```

Ensuite dans mon container mysql je me suis connecté puis j'ai crée la table e-bakery, voici les 2 commandes :
- ```docker exec -it mon_container_mysql mysql -uroot -p```
- ```CREATE DATABASE 'e-bakery';```

Et enfin, j'ai démarré démarré mon container : ```docker run -d -p 8080:80 --name container_api api-e-bakery```

Et on peut voir que ça fonctionne :
![alt text](images/1.png)

![alt text](images/2.png)

## 5. Créer un docker-compose.yml pour avoir 2 services (node et db)
J'ai crée le docker-compose :
```
version: '3'
 
services:
  db:
    image: mysql:latest
    container_name: db
    environment:
      MYSQL_ROOT_PASSWORD: mdp
    ports:
      - "3000:3306"
    volumes:
      - tp3_data:/var/lib/mysql
  node:
    build: .
    container_name: node
    links:
      - db
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
    restart: always
    ports:
      - 8080:80
volumes:
  tp3_data:
```

## 6. Faire les adaptations nécessaires au docker-compose pour que votre app puisse utiliser votre base de données conteneurisée