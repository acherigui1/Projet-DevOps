

## Ecrivez ici les inscriptions et explications pour déployer l'infrastructure et l'application sur Azure
Le cluster AKS est l'environnement où votre application Flask sera exécutée. Il gère les conteneurs de l'application et orchestre leur distribution et leur échelle.

L'Azure Container Registry (ACR) est utilisé pour stocker des images Docker, qui sont des snapshots de votre application Flask contenant le code et les dépendances nécessaires pour l'exécuter

Construire l'image Docker :

shell

docker build -t <nom_image>:<tag> .

Authentifiez-vous à votre ACR :

shell

az acr login --name <nom_registry>

Tagger l'image pour ACR :

shell

docker tag <nom_image>:<tag> <nom_registry>.azurecr.io/<nom_image>:<tag>

Envoyer l'image sur ACR :

shell

docker push <nom_registry>.azurecr.io/<nom_image>:<tag>

//

dans le dossier Terraform :
cd terraform
terraform init
terraform plan
terraform apply

dans le dossier Flask-app :
cd flask-app
az acr login --name osamiwcontainerregistry
docker tag examdevops:iw2 osamiwcontainerregistry.azurecr.io/examdevops:iw2
docker push osamiwcontainerregistry.azurecr.io/examdevops:iw2
az aks update -n devopsAKSCluster -g rg-amine-cherigui-cc --attach-acr osamiwcontainerregistry

dans le dossier kubernetese :
cd kubernetes
az aks get-credentials --resource-group rg-amine-cherigui-cc --name devopsAKSCluster
kubectl apply -f deployment.yaml 
kubectl apply -f service.yaml 
kubectl apply -f redis-service.yaml 
kubectl apply -f redis-deployment.yaml