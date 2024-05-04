# TP3
### Fait par Yann Sady

## 2. Créer un Dockerfile qui permet de lancer une application NodeJS (v18-alpine ou v20)

J'ai crée le Dockerfile :
```
FROM node:18-alpine

WORKDIR /home/node/app

COPY ./e-bakery/ ./

RUN npm install

EXPOSE 80

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
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
  node:
    build: .
    container_name: node
    links:
      - db
    restart: always
    ports:
      - 8080:80
      
volumes:
  tp3_data:
```

## 6. Faire les adaptations nécessaires au docker-compose pour que votre app puisse utiliser votre base de données conteneurisée

Malgré le fait que j'ai mis un lien à db dans le docker-compose, le container node redémarre en boucle :
![alt text](images/3.png)

Et c'est du à une erreur de connection avec la base de données :
![alt text](images/4.png)

Mais nous rétablirons le lien dans l'étape 8

## 7. Utilisez des variables d'environnement dans votre docker-compose ET adaptez l'application (db.config.js) pour utiliser ces variables

D'abord j'ai crée les variables dans .env :  
![alt text](images/5.png)

Ensuite je les ai mise dans config.js :  
![alt text](images/6.png)

Puis dans mon docker-compose :  
![alt text](images/7.png)  
(ici j'ai ajouté à node environment pour ajouter les variables d'environnement)

## 8. Faites en sorte d'isoler vos 2 services docker-compose sur le même network.

Après avoir modifier mon docker-compose :  
```
version: '3'
 
services:
  node:
    build: .
    container_name: node
    networks:
      - tp3-network
    depends_on:
      - db
    environment:
      DB_USERNAME: ${DB_USERNAME}
      DB_PASSWORD: ${DB_PASSWORD}
      DB_NAME: ${DB_NAME}
      DB_HOST: ${DB_HOST}
      DB_DIALECT: ${DB_DIALECT}
      PORT: ${PORT}
    restart: always
    ports:
      - 8080:80
  db:
    image: mysql:latest
    container_name: db
    networks:
      - tp3-network
    environment:
      MYSQL_ROOT_PASSWORD: ${DB_PASSWORD}
    ports:
      - "3306"
    volumes:
      - tp3_data:/var/lib/mysql
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql

networks:
  tp3-network:
    driver: bridge
    name: tp3-network

volumes:
  tp3_data:
```

On peut voir que cela fonctionne bien :  
![alt text](images/8.png)  
![alt text](images/9.png) 