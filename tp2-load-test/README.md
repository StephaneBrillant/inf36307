## Deployer locust loadtest dans kubernetes
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
