# Kubernetes Project for Ynov - Groupe 4

Ce repo est la partie Helm pour avoir un déploiement plus simple et plus rapide

Le repo de base et où tout se lance à la mano est ici : https://github.com/Louispautasso/kube-groupe4
Ce projet a été créer dans le but de fonctionner sur un Cluster Kubernetes. Ce cluster pourra être installer sur un cloud provider comme sur un cluster kube local.

# Pour Tout installer

## Les pré-requis

Il va falloir installer helm sur le cluster Kube
https://helm.sh/docs/intro/install/

Puis nous allons ajouter un controller d'ingress, pour cela, nous allons utiliser Nginx
On commence par créer son propre namespace
```bash
kubectl create namespace ingress-projet
```

On ajoute le repo ingress-nginx et on le met à jour 
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```

Puis on install le controller Ingress 
```bash
helm install nginx-ingress ingress-nginx/ingress-nginx --namespace ingress-projet
```
## Installation du projet

On commence par créer son namespace
```bash
kubectl create namespace projet
```

Puis on installe l'ensemble du projet via la commande helm 
```bash
helm install projet ./helm-charts/projet --namespace projet
```

## Installation du monitoring des metrics - Prometheus / Grafana

On commence par créer son namespace
```bash
kubectl create namespace monitoring
```

Puis on ajoute le repo helm et on l'update
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

Puis on lance l'installation du Chart Helm avec notre fichier de valeurs et dans le namespace qu'on souhaite
```bash
helm install prometheus prometheus-community/kube-prometheus-stack -f helm-charts/prometheus/values.yml --namespace monitoring
```



## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)