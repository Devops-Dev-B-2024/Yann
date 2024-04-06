# TP2
### Fait par Yann Sady

## 3. Exécuter un serveur web (apache, nginx) dans un container docker

a. ```docker pull nginx```

b. ![alt text](images/1.png)

c. Fichier crée dans le dossier html

d. ```docker run -d -p 8080:80 --name container_hello_world -v "$(pwd)/html/index.html:/usr/share/nginx/html/index.html" nginx```
On peut voir index.html :
![alt text](images/2.png)

e. ```docker rm container_hello_world```

f. Je relance le container sans -v : ```docker run -d -p 8080:80 --name container_hello_world nginx```

Puis je copie le chemin de mon dossier en local dans mon container : ```docker cp "$(pwd)/html/index.html" container_hello_world:/usr/share/nginx/html/index.html```
Et on peut voir que cela fonctionne :
![alt text](images/3.png)

## 4. Builder une image

a. J'ai crée le dossier Dockerfile puis j'ai fait la commande : ```docker build -t mon-serveur-nginx .``` pour build mon image.

b. J'exécute ma nouvelle image via la commande ```docker run -d -p 8080:80 mon-serveur-nginx```
Voici mon index.html :
![alt text](images/4.png)

c. Dans la question 3, on a utilisé le montage de volume avec -v alors que dans la question 4, on a directement copié le fichier index.html dans l'image que l'on a crée avec le Dockerfile.

Pour le mount volume :
- Avantages : Plus simple à mettre en place car il n'y a pas besoin de créer de Dockerfile
- Inconvénients : Commandes longues à taper

Pour la copy :
- Avantages : Moins de commande à taper et donc plus rapide à exécuter
- Inconvénients : Plus complexe à mettre en place car il faut créer un Dockerfile et connaitre la syntaxe pour le faire

## 5. Utiliser une base de données dans un container docker

a. Mysql : ```docker pull mysql``` et phpmyadmin : ```docker pull phpmyadmin```

b. J'ai exécuté :
- ```docker run -d -p 8081:80 --name mon_container_mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=yes mysql``` 
- ```docker run -d -p 8080:80 --name mon_container_phpmyadmin phpmyadmin```

Ensuite il faut que l'on connecte les 2 containers via un network, pour commencer j'ai crée un network ```docker network create reseau_bdd```

Puis j'ai ajouté mes 2 containers dans ce network :
- ```docker network connect reseau_bdd mon_container_mysql```
- ```docker network connect reseau_bdd mon_container_phpmyadmin```

Ensuite pour que mon container phpmyadmin utilise le nom d'hôte pour mettre mon container mysql
![alt text](images/5.png)

J'ai crée une bdd nommé test_bdd puis une table via l'interface :
![alt text](images/6.png)

Et on peut voir qu'elle a bien été crée :
![alt text](images/7.png)