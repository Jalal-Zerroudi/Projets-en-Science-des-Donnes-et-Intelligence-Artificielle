# Détection de fraude bancaire

> **État actuel : projet planifié, non implémenté dans ce dépôt.**

## Objectif

Détecter les transactions potentiellement frauduleuses dans un jeu de données fortement déséquilibré, en privilégiant la détection des fraudes sans ignorer le coût des faux positifs.

## Contenu disponible

Ce dossier ne contient actuellement ni code source, ni notebook, ni jeu de données, ni modèle entraîné. Les approches et scores mentionnés dans le [README principal](../README.md) restent des objectifs tant qu'un protocole reproductible n'est pas versionné.

## Livrables attendus

- procédure d'accès au jeu de données respectant sa licence ;
- analyse du déséquilibre des classes et baseline ;
- pipeline évitant toute fuite de données pendant le rééchantillonnage ;
- fichier de dépendances avec versions ;
- évaluation avec précision, rappel, F1, PR-AUC et matrice de confusion ;
- seuil de décision documenté et analyse des faux positifs ;
- script d'inférence sur un exemple anonymisé.
