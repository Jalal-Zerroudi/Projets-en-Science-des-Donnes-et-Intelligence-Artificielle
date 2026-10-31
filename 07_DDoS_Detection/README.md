# Détection d'attaques DDoS

> **État actuel : projet planifié, non implémenté dans ce dépôt.**

## Objectif

Détecter des flux réseau associés à des attaques DDoS et mesurer la capacité du modèle à généraliser sans fuite entre les jeux de données.

## Contenu disponible

Ce dossier ne contient actuellement ni code source, ni notebook, ni données réseau, ni modèle entraîné. Les modèles et scores du [README principal](../README.md) sont des objectifs non encore reproduits ici.

## Livrables attendus

- procédure d'accès au corpus et description des classes ;
- séparation des données par capture, période ou scénario avant prétraitement ;
- baseline classique avant tout modèle profond ;
- fichier de dépendances et paramètres d'entraînement ;
- métriques par classe, F1, rappel DDoS, faux positifs et matrice de confusion ;
- mesure du temps d'inférence et des ressources utilisées ;
- analyse des limites de généralisation à d'autres réseaux.
