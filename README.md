# Table des Matières
<!-- TOC -->

- [Table des Matières](#table-des-mati%C3%A8res)
- [Détection de Places de Parking Libres par Images Satellitaires](#d%C3%A9tection-de-places-de-parking-libres-par-images-satellitaires)
    - [Aperçu du Projet](#aper%C3%A7u-du-projet)
    - [Création du Jeu de Données](#cr%C3%A9ation-du-jeu-de-donn%C3%A9es)
    - [Développement du Modèle](#d%C3%A9veloppement-du-mod%C3%A8le)
    - [Prochaines Étapes et Tests](#prochaines-%C3%A9tapes-et-tests)
    - [Instructions d'Exécution](#instructions-dex%C3%A9cution)
    - [Instructions d'utilisation de l'API](#instructions-dutilisation-de-lapi)
    - [Bibliographie](#bibliographie)
- [Revue de code](#revue-de-code)
- [Contribuer](#contribuer)
    - [Cas Spécifique](#cas-sp%C3%A9cifique)
- [Évaluation](#%C3%A9valuation)
    - [Modalités](#modalit%C3%A9s)
- [Checklist des bonnes pratiques de développement](#checklist-des-bonnes-pratiques-de-d%C3%A9veloppement)
- [Parcours MLOps](#parcours-mlops)
        - [> ### Objectif](#--objectif)
    - [Etapes :](#etapes-)
- [Séparation code Data](#s%C3%A9paration-code-data)

<!-- /TOC -->
# Détection de Places de Parking Libres par Images Satellitaires

## Aperçu du Projet
Ce projet vise à développer un modèle d'apprentissage automatique pour détecter les places de parking libres à partir d'images satellitaires. Initié par Maxime RICHAUDEAU et Yorik NYSSEN, l'inspiration est venue des difficultés rencontrées pour trouver des places de parking en arrivant à destination en utilisant des applications de navigation GPS comme Waze.

## Création du Jeu de Données

Le jeu de données comprend 120 images satellitaires de différents lieux en Europe, annotées manuellement pour marquer les places de parking libres. Le jeu de données a été divisé en ensembles de training, de validation et de test, avec l'application de techniques d'augmentation pour augmenter la taille et la diversité du jeu de données.

## Développement du Modèle

Un modèle U-Net a été choisi pour cette tâche de segmentation, initialement ajusté à partir d'un état pré-entraîné puis entraîné à partir de zéro avec ajustement des poids de classe pour améliorer la performance. Le modèle final a montré des résultats prometteurs dans la segmentation des routes et des zones pavées mais a eu du mal à identifier avec précision les places de parking libres.

## Prochaines Étapes et Tests

Des suggestions d'amélioration incluent l'extension de la durée d'entrainement, l'enrichissement du jeu de données et l'ajustement des hyperparamètres. Des instructions de test et des références bibliographiques sont fournies pour ceux intéressés à exécuter le projet.

## Instructions d'Exécution

Pour l'exécution du projet, suivez les étapes préparatoires décrites dans les notebooks Jupyter fournis pour la création de masques, le redimensionnement des images et l'application de l'augmentation de données, suivies par la formation et le test du modèle comme détaillé dans le notebook `Train_Test.ipynb`.

## Instructions d'utilisation de l'API

Il est possible d'utiliser le modèle afin de réaliser des prédictions des places de parking. Pour cela, une API est mise à disposition dans le script `Main.py`. Pour utiliser l'API en local, il suffit dans le terminal de se placer dans le dossier contenant celle-ci et d'entrer la commande suivante : 
```{r, engine='bash', count_lines}
 uvicorn main:app --reload
```
Une fois l'API lancée il suffit d'entrer l'adresse qui s'affiche dans le terminal dans un navigateur web et la page d'accueil s'affichera. Afin de réaliser une prédiction, il suffit de coller une url dans la zone de texte et de cliquer sur le bouton 'Show Prediction'

## Bibliographie

1. [Smith, J. (2020). "Efficient Image Segmentation using PyTorch - Part 1." Towards Data Science.](https://towardsdatascience.com/efficient-image-segmentation-using-pytorch-part-1-89e8297a0923)
2. [Smith, J. (2020). "Efficient Image Segmentation using PyTorch - Part 2." Towards Data Science.](https://towardsdatascience.com/efficient-image-segmentation-using-pytorch-part-2-bed68cadd7c7)
3. [Smith, J. (2020). "Efficient Image Segmentation using PyTorch - Part 3." Towards Data Science.](https://towardsdatascience.com/efficient-image-segmentation-using-pytorch-part-3-3534cf04fb89)
4. [Smith, J. (2020). "Efficient Image Segmentation using PyTorch - Part 4." Towards Data Science.](https://towardsdatascience.com/efficient-image-segmentation-using-pytorch-part-4-6c86da083432)
5. [Albumentations.](https://albumentations.ai/)
6. [Albumentations. (Documentation). "PyTorch Semantic Segmentation."](https://albumentations.ai/docs/examples/pytorch_semantic_segmentation/)
7. [TernausNet GitHub Repository.](https://github.com/ternaus/TernausNet/tree/master)
8. [PyTorch Tutorials. "Data Loading and Processing Tutorial."](https://pytorch.org/tutorials/beginner/data_loading_tutorial.html)

# Revue de code
Lors de la revue de code, si vous souhaitez utiliser l'API mise en ligne il sera surement nécessaire de rédéployer l'ensemble de la solution car 
des fichiers d'environnement contenant les tokens d'accès aux données situées sur le stockage S3 du SSPCloud devront être renouveler, ce qui nécessite un redéploiement.  
Dans ce cas, vous pouvez envoyer un mail à maxime.richaudeau@ensae.fr afin de réaliser cette tâche et qu'il puisse vous transmettre le lien web de l'API une fois déployée.

Le projet a besoin d'un fichier .env pour fonctionner en local. Il contient notamment les secrets pour donner les accès à l'api au stockage S3 de maximerichaudeau1. Il est de la forme suivante.
```code
AWS_S3_ENDPOINT = minio.lab.sspcloud.fr
AWS_ACCESS_KEY_ID = <your_acces_key_id>
AWS_SECRET_ACCESS_KEY = <your_secret_access_key>
AWS_SESSION_TOKEN = <your_session_token>
```

# Contribuer 

## Cas Spécifique

Sur les machines d'Onixia j'ai rencontré une erreur avec l'import de la librairie **opencv-python** qui ressemblait à cette erreur : 

```code
    import cv2
  File "/opt/mamba/lib/python3.11/site-packages/cv2/__init__.py", line 181, in <module>
    bootstrap()
  File "/opt/mamba/lib/python3.11/site-packages/cv2/__init__.py", line 153, in bootstrap
    native_module = importlib.import_module("cv2")
                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/mamba/lib/python3.11/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ImportError: libGL.so.1: cannot open shared object file: No such file or directory
```

L'erreur que nous rencontrons, ImportError: libGL.so.1: cannot open shared object file: No such file or directory, se produit généralement lorsque les bibliothèques OpenGL requises ne sont pas installées sur notre système. Cette bibliothèque est nécessaire pour que OpenCV puisse rendre des images et des vidéos, car il s'appuie sur l'accélération matérielle graphique fournie par OpenGL. Pour y remédier : 

```bash
sudo apt-get update
sudo apt-get install libgl1-mesa-glx
```

> **⚠️ Attention**
>
> Il faudra donc surement ajouter cette commande au Dockerfile afin d'installer la librairie supplémentaire.


# Évaluation

**Modalités d'évaluation du cours**

## Modalités

L'objectif général de l'évaluation de ce cours est de **mettre en pratique les notions étudiées** de manière appliquée et réaliste, à travers un projet basé sur une problématique "métier" et des données réelles. Pour cela, l'évaluation sera en deux parties :

1. **En groupe de 3** : un projet à choisir parmi les 3 parcours (MLOps, application interactive / tableau de bord, publication reproductible + site web). Idéalement, on choisira un projet réel, effectué par exemple dans le cadre d'un cours précédent et qui génère un résultat propre à une mise en production.
2. **Individuellement** : effectuer une revue de code d'un autre projet. Compétence essentielle et souvent attendue d'un data scientist, la revue de code sera l'occasion d'intégrer les bonnes pratiques de développement et de faire un retour constructif sur un projet autre que celui de son groupe.

> **⚠️ Attention**
>
> Ce projet doit mobiliser des données publiquement accessibles. La récupération et structuration de ces données peut faire partie des enjeux du projet mais celles-ci ne doivent pas provenir d'un projet antérieur de votre scolarité pour lequel le partage de données n'est pas possible.

# Checklist des bonnes pratiques de développement

Les bonnes pratiques de développement ci-dessous sont les **indispensables de ce cours**. Elles doivent être à la fois appliquées dans les **projets de groupe**, et à la base de la **revue de code individuelle**.

- [x] **Utilisation de Git**
  - Présence d’un fichier `.gitignore` adapté au langage et avec des règles additionnelles pour respecter les bonnes pratiques de _versioning_
  - Travail collaboratif : utilisation de branches et des _pull requests_
- [x] **Présence d’un fichier `README`** présentant le projet : 
  - [x] contexte
  - [x] objectif
  - [x] comment l’utiliser : voir la partie revue de code.
- [x] **Présence d’un fichier `LICENSE`** déclarant la licence (_open-source_) d’exploitation du projet.
- [x] **Versioning des packages** : présence d’un fichier `requirements.txt` ou d’un fichier d’environnement `environment.yml` pour conda

- [x] **Qualité du code**
  - [x] Respect des standards communautaires : utiliser un _linter_ et/ou un _formatter_
  - [x] Modularité : un script principal qui appelle des modules

- [ ] **Structure des projets**
  - [ ]Respect des standards communautaires (_cookiecutter_)
  - [x] Modularité du projet selon le modèle évoqué dans le cours:
    - [x] Code sur GitHub
    - [x] Données sur S3
    - [x] Fichiers de configuration (_secrets_, etc.) à part

# Parcours MLOps

### > ### Objectif
> 
> A partir d’un projet existant ou d’un projet type _contest Kaggle_, développer un modèle de ML répondant à une problématique métier, puis le déployer sur une infrastructure de production conformément aux principes du **MLOps**.

## Etapes :

- [x] **Respecter la _checklist_ des bonnes pratiques de développement** ;
- [x] **Développer un modèle de ML qui répond à un besoin métier** : Nous avons repris un ancien projet;
- [ ] **Entraîner le modèle via validation croisée, avec une procédure de _fine-tuning_ des hyperparamètres** ;
- [ ] **Formaliser le processus de _fine-tuning_ de manière reproductible via MLFlow** ;
- [x] **Construire une API avec Fastapi pour exposer le meilleur modèle** ;
- [x] **Créer une image Docker pour mettre à disposition l'API** ;
- [x] **Déployer l'API sur le SSP Cloud** ;
- [x] **Industrialiser le déploiement en mode GitOps avec ArgoCD** ;
- [ ] **Gérer le monitoring de l'application : _logs, dashboard_ de suivi des performances, etc.**


# Séparation code Data 
```bash
mc mb s3/maximerichaudeau1/data
mc cp --recursive ./data/ s3/maximerichaudeau1/data

mc mb s3/maximerichaudeau1/json
mc cp --recursive ./json/ s3/maximerichaudeau1/json

mc cp ./cross_entropy_weighted10_batch64_32_16.pth  s3/maximerichaudeau1/cross_entropy_weighted10_batch64_32_16.pth
```
