Prérequis
Pour travailler sur ce projet, assurez-vous d'avoir les outils suivants installés et configurés sur votre environnement :
1/ Docker
2/Jenkins
3/Kubernetes
4/Helm
5/ArgoCD
6/Promethius 
7/Grafana
8/Kubectl
9/Git

Etapes de Réalisation : 

1. Création d'un fichier Dockerfile
Backend (API) : Créez un fichier Dockerfile dans le dossier Backend.
cet fichier contient les instructions nécessaires pour conteneuriser leurs composants respectifs.

2. Configuration avec Docker Compose :
   
Créez un fichier docker-compose.yml à la racine du projet pour orchestrer les conteneurs backend et base de Donnée.
Vérifiez que les services communiquent correctement.

3. Mise en place du CI avec Jenkins :

. Pipeline CI

. Créez un fichier Jenkinsfile contenant les étapes suivantes :

. Start : Initialisation de la pipeline.

. Checkout SCM : Récupération du code source depuis le dépôt.

. Build Server Image : Construction de l'image Docker pour le Backend.

. Test Docker Hub Connectivity : Vérification de la connexion au Docker Hub.

. Push Images to Docker Hub : Publication des images sur Docker Hub.

. End : Fin de la pipeline.

4. Mise en place du CD avec Kubernetes:

. Manifests simples

. Créez les fichiers suivants dans le dossier kubernetes :

. devops-deployment.yaml : Déploiement de l'API.

. devops-service.yaml : Service exposant l'API avec un NodePort.

. Déploiement local

. Appliquez les manifests dans le cluster local : kubectl apply -f kubernetes/

. Vérifiez que le service est accessible via l'adresse du NodePort.

. Déploiement avec Helm et ArgoCD

. Créez des Helm charts pour chaque composant dans le dossier helm-charts.

. Configurez ArgoCD pour suivre ces charts et gérer les déploiements.

. Vérifiez que les applications sont Healthy et Sync dans l'interface ArgoCD.

5. Monitoring avec Prometheus et Grafana :

. Installez Prometheus et Grafana dans le namespace monitoring.

. Configurez Prometheus pour collecter les métriques des composants de l'application.

. Créez un tableau de bord dans Grafana pour visualiser ces métriques.

Résultat final

Notre Application est déployée localement et dans un namespace avec Helm.

Les pipelines CI/CD fonctionnent avec Jenkins et ArgoCD.

Le monitoring est opérationnel avec des métriques visibles dans Grafana.
