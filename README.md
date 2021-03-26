# Kubernetes Project for Ynov - Groupe 4

Ce repo est la partie Helm pour avoir un déploiement plus simple et plus rapide

Le repo de base et où tout se lance à la mano est ici : https://github.com/Louispautasso/kube-groupe4

Ce projet a été créer dans le but de fonctionner sur un Cluster Kubernetes. Ce cluster pourra être installer sur un cloud provider comme sur un cluster kube local.

![alt text](https://github.com/Louispautasso/kube-ynov/blob/others/diagramme.png?raw=true)

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

## Installation du monitoring des logs - EFK

On commence par créer son namespace
```bash
kubectl create namespace kube-logging
```

Puis on lance l'installation du Chart Helm  d'ElasticSearch dans le namespace kube-logging
```bash
helm install elasticsearch ./helm-charts/elasticsearch --namespace kube-logging
```

Puis on lance l'installation du Chart Helm  de Kibana dans le namespace kube-logging
```bash
helm install kibana ./helm-charts/kibana --namespace kube-logging
```

Puis on lance l'installation du Chart Helm de Fluentd dans le namespace kube-logging
```bash
helm install fluentd ./helm-charts/fluentd --namespace kube-logging
```

## Installation du certificat auto-signé

```bash
kubectl create secret tls projet-1 --cert certificats/projet-1.crt --key certificats/projet-1.key
```


# Pour les noobs
# Quelques commandes
Voir les releases qui tournent actuellement et les supprimer
```bash
helm list -A
helm uninstall <release_name> -n <namespace>
```

Se connecter à son cluster Azure
```bash
az aks get-credentials --resource-group groupe4 --name clus4
```

Utilisation des contexts
```bash
kubectl config get-contexts
kubectl config use-context
```
# Quelques liens
Création de la partie monitoring des metrics avec Prometheus
```bash
https://medium.com/codex/setup-kuberhealthy-with-prometheus-and-grafana-on-minikube-b2f6da21dc2e
```
Création des sondes de vivacité et de préparation
```bash
https://linuxintosh.wordpress.com/2020/12/28/k8s-les-differents-types-de-sondes/
```
Création des règles d'affinités
```bash
https://docs.openshift.com/container-platform/3.6/admin_guide/scheduling/pod_affinity.html
https://stackoverflow.com/questions/62292476/deployment-affinity
```
Création de la partie monitoring des logs avec EFK + pas mal de recherche pour des petits bug
```bash
https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes#step-2-%E2%80%94-creating-the-elasticsearch-statefulset
```

## Contributing

Florian Agnès and Louis Pauatasso les proprios.

## License
C'est à nous