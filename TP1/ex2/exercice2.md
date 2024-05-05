# Exercice 2
### Fait par Yann Sady

## Bonne pratiques git
Les bonnes pratiques sont :
- d'avoir une branche master/main protégé (autorisé que les pull request mais pas le merge direct ou encore de push dedans)
- travailler sur des branches différentes en fonction des dev/features
- protéger les branches différentes (dev/features) pour n'autorisé que les commits et push des personnes travaillant dessus
- commit et push au fur et à mesure que les tâches sont finies
- mettre des commits descriptif et pas général/flou
- ne pas permettre d'outrepassé les protections mêmes pour l'admin

## Application des bonnes pratiques
J'ai commencé par protégé ma branche main :  
![alt text](images/1.png)   
![alt text](images/3.png)  
  
Et j'ai également protégé ma branche de travail (ici tp1) :  
![alt text](images/4.png)  
  
Quand moi ou mon acolyte essayons de push dans main ou que mon acolyte essait de push dans la branche tp1 cela affiche bien une erreur et ne le permet pas :  
![alt text](images/5.png)  
  
Et maintenant si on fait une pull request il faudra qu'un developpeur approuve les changements pour pouvoir merge :  
![alt text](images/6.png)  
  
Une fois les changements approuvé on peut merge :  
![alt text](images/7.png)  
