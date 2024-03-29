### TP2
# Fait par Yann Sady

## 3. Exécuter un serveur web (apache, nginx) dans un container docker

a. docker pull nginx

b. ![alt text](images/1.png)

c. Fichier crée dans le dossier html

d. docker run -d -p 8080:80 --name container_hello_world -v "$(pwd)/html/index.html:/usr/share/nginx/html/index.html" nginx
On peut voir index.html :
![alt text](images/2.png)

e. docker rm container_hello_world

f. Je relance le container sans -v : docker run -d -p 8080:80 --name container_hello_world nginx

Puis je copie le chemin de mon dossier en local dans mon container : docker cp "$(pwd)/html/index.html" container_hello_world:/usr/share/nginx/html/index.html
Et on peut voir que cela fonctionne :
![alt text](images/3.png)
