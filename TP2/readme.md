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