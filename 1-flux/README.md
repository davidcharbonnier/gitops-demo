# Weave Flux

Table des matières:

1. [Installation de Flux](#Installation-de-flux)
  1. [Prérequis](#Prérequis)
  2. [Installation](#Installation)
  3. [Configuration](#Configuration)
2. [Syncronisation initiale et déploiement des applicatifs](#Syncronisation-initiale-et-déploiement-des-applicatifs)
3. [Mise à jour applicative](#Mise-à-jour-applicative)

## Installation de Flux

### Prérequis

Il est nécessaire d'avoir un cluster Kubernetes fonctionnel (minikube, kind, kubeadm ou managé type GKE/EKS/AKS).

En plus cela, il vous faudra installer le binaire [`fluxctl`](https://docs.fluxcd.io/en/1.19.0/references/fluxctl/).

Enfin, comme nous allons utiliser Helm pour installer Flux et Helm Operator, il est nécessaire d'installer [`helm`](https://helm.sh/docs/intro/install/) v3.

### Installation

L'installation que nous réalisons comprend Flux ainsi que Helm Operator, permettant à Flux de déployer des chart Helm en plus de simples manifests.

Préparation à l'installation de Flux:

```bash
helm repo add fluxcd https://charts.fluxcd.io
kubectl apply -f https://raw.githubusercontent.com/fluxcd/helm-operator/master/deploy/crds.yaml
kubectl create namespace flux
```

Installation de Flux:

```bash
helm upgrade -i flux fluxcd/flux --set git.url=git@github.com:davidcharbonnier/gitops-demo --set git.path=1-flux --namespace flux
```

Installation de Helm Operator:

```bash
helm upgrade -i helm-operator fluxcd/helm-operator --set git.ssh.secretName=flux-git-deploy --set helm.versions=v3 --namespace flux
```

Vérification de l'installation:

```bash
watch kubectl -n flux get pods
```

### Configuration

Récupération de la clé publique SSH de Flux (pour qu'il puisse écrire dans le repo):

```bash
fluxctl identity --k8s-fwd-ns flux
```

Il faut maintenant ajouter cette clé SSH comme deploy key au sein du repository et lui donner les droits d'écriture, cela permettra à Flux d'effectuer les modifications aux manifests présents dans le repository.

## Syncronisation initiale et déploiement des applicatifs

TBD

## Mise à jour applicative

TBD
