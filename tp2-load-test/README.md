# Travail Pratique 2
## Gestion de grande charge

### Context
Ce dossier comprends des fichiers de configuration kubernetes qui vous permetterons de:
* Lancer un server de Base de Données `MySQL`
* Lancer un webservice REST utilisant la Base de Données `MySQL`
* Lancer un simulateur de charge `locust`

Le simulateur de charge est préconfiguré pour générer du traffic sur le weservice REST.

### Objectifs
* Obtenir le plus grands nombre possible de Requête Par Secondes (RPS)
* Documenter toutes vos experimentations et leurs résultats par écrit

### Limitation
Il est interdit de modifier les images utilisées pour le webservice, la base de données ou le générateur de charge.
Vous pouvez toutefois changer tous les fichiers de configuration kubernetes à votre guise.

## Deployer locust loadtest dans kubernetes avec les serveurs
kubectl apply -f loadtest/.  
kubectl apply -f server/.  

## Tester le webUI locust
minikube service list
minikube service locust-master

## Comment detruire un deployment
kubectl delete -f loadtest/.
kubectl delete -f server/.  

## Comment detruire un volume persistant?
kubectl get persistentvolume
kubectl delete persistentvolume pvc-0c78d78a-205e-40db-9e0d-9100b236feb8

## Comment voir les metrics d'utilisation des ressources?
minikube addons enable metrics-server
kubectl -n kube-system rollout status deployment metrics-server

## Comment démarrer minikube avec plus de ressource
minikube delete
minikube start --cpus 8 --memory 8096

## Comment utiliser gatling au lieu de locust?
Gatling n'a pas de webUi durant l'execution des tests, mais produit un rapport à la fin d'une simulation.
Les résultats seront disponibles dans le dossier loadtest-gatling/results en format html

### Comment lancer une simulation gatling?
Obtenir le port pour accèder a votre webservice via :
minikube service werservice

Configurer votre simulation en modifiant le fichier /loadtest-gatling/user-files/simulations/webservice.scala
Ligne 11 - Remplacer le port par le port de votre service
Ligne 20 à 27 - Modifier à votre guise les paramètres de la simulation. Des exemples sont fournis en commentaires

Lancer la simulation
docker run -it --rm  --network host -v "$(pwd)/loadtest-gatling/conf:/opt/gatling/conf" -v "$(pwd)/loadtest-gatling/user-files:/opt/gatling/user-files" -v "$(pwd)/loadtest-gatling/results:/opt/gatling/results" denvazh/gatling:2.3.0 -s webservice

À la fin de la simulation, consultez les résultats
