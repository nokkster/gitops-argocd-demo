# GitOps avec ArgoCD sur Kubernetes

## Objectif

L'objectif de ce projet est de découvrir l'approche GitOps dans un environnement Kubernetes à l'aide d'ArgoCD.

Le projet consiste à déployer une application nginx dans Kubernetes puis à automatiser sa gestion à partir d'un dépôt Git.

---

## Technologies utilisées

- Kubernetes
- Minikube
- ArgoCD
- GitHub
- kubectl

---

## Architecture

GitHub
    |
    v
ArgoCD
    |
    v
Kubernetes
    |
    v
Pods nginx

---

## Fichiers du projet

- namespace.yaml
- deployment.yaml
- service.yaml
- app.yaml

---

## Installation

### Création du cluster

```
minikube start
```
### Installation d'ArgoCD
```
kubectl create namespace argocd

kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Déploiement de l'application
```
kubectl apply -f app.yaml
```

### Démonstration GitOps

Version initiale :
```
replicas: 1
```

Modification :
```
replicas: 3
```

Après le commit GitHub, ArgoCD détecte automatiquement le changement et met à jour le cluster Kubernetes.

Rollback :
```
replicas: 1
```

ArgoCD synchronise automatiquement le cluster.

---

## Pipeline CI/CD

Le projet contient une pipeline GitHub Actions située dans :

.github/workflows/validate-k8s.yml

Cette pipeline est exécutée à chaque push ou pull request.

Elle réalise deux contrôles :

1. Vérification de la syntaxe YAML des fichiers du dépôt.
2. Validation des manifests Kubernetes avec kubeconform.

Dans cette architecture, la CI ne déploie pas directement dans Kubernetes.  
Le déploiement est assuré par ArgoCD selon le principe GitOps.

Ainsi :

- GitHub Actions = validation CI
- ArgoCD = déploiement CD
- GitHub = source de vérité

---

## Résultats

* Déploiement automatique
* Synchronisation Git → Kubernetes
* Gestion des versions
* Rollback simplifié

---

## Auteur

Bohdan Dyshlevyy
