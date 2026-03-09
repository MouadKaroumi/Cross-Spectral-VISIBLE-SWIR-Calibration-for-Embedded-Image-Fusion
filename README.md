
# Calibration entre capteurs RGB et SWIR

## Idée générale du projet

L'objectif de ce projet est de réaliser la **calibration entre deux capteurs d'images hétérogènes** : un capteur RGB (visible) et un capteur SWIR (infrarouge à ondes courtes). La calibration vise à trouver une transformation géométrique qui permet d'aligner spatialement les images issues de ces deux capteurs, afin de pouvoir exploiter conjointement leurs informations pour des applications de fusion, détection ou analyse multi-spectrale.

## Démarche : Homographie généralisée

La démarche adoptée consiste à **trouver une matrice d'homographie généralisée** qui aligne au mieux l'ensemble des paires d'images RGB/SWIR acquises simultanément. Plutôt que de calculer une homographie spécifique à chaque paire, on cherche une transformation unique, robuste et généralisable à toutes les données du jeu d'apprentissage. Cette homographie est optimisée pour minimiser une mesure de dissimilarité (MIND) entre les images alignées.

Le pipeline exploite le modèle RoMa pour obtenir une estimation initiale des correspondances, puis affine la solution par optimisation globale.

## Explication du pipeline

1. **Importation des librairies** : Chargement des modules nécessaires pour le traitement d'images, l'optimisation, la manipulation de fichiers et la visualisation.

2. **Configuration** : Tous les paramètres (chemins, tailles, hyperparamètres) sont centralisés dans la classe `Config` pour faciliter l'adaptation à d'autres jeux de données.

3. **Pairing des images** : Les images RGB et SWIR sont associées automatiquement en fonction de leur timestamp pour garantir qu'elles proviennent du même instant de capture.

4. **Prétraitement** : Les images sont chargées, redimensionnées à une taille standard, et des masques de validité sont générés pour ne conserver que les régions pertinentes.

5. **Calcul du descripteur MIND** : Le descripteur MIND (Modality Independent Neighborhood Descriptor) est utilisé pour quantifier la similarité locale entre images alignées, indépendamment de la modalité (visible/infrarouge).

6. **Initialisation RoMa** : Le modèle RoMa (Robust Matcher) est utilisé pour estimer les correspondances initiales entre chaque paire d'images et calculer une homographie par RANSAC.

7. **Estimation des homographies** : Pour chaque paire, une homographie initiale est calculée. Les homographies invalides sont écartées.

8. **Optimisation globale** : Une homographie générale est initialisée (médiane des homographies valides), puis optimisée globalement sur toutes les paires pour minimiser la perte MIND, avec régularisation et validation croisée pour éviter le sur-apprentissage.

9. **Sauvegarde** : La matrice d'homographie généralisée finale est sauvegardée au format `.npy` et `.json` pour une utilisation ultérieure.

10. **Visualisation** : Le notebook propose des visualisations pour évaluer la qualité de l'alignement (avant/après, fusion HSV, heatmap d'erreur MIND).


## Dépendances
Assurez-vous d'installer toutes les dépendances listées dans `requirements.txt` avant d'exécuter les scripts ou notebooks.

## Utilisation

- Modifiez les chemins et paramètres dans la classe `Config` selon vos données.
- Assurez-vous que le modèle RoMa est installé et accessible.
- Exécutez le notebook étape par étape pour générer et sauvegarder l'homographie.
- Utilisez la visualisation pour vérifier la qualité de l'alignement.

## Contact

Pour toute question ou amélioration, contactez-moi à : karoumimouad@gmail.com

---

© 2026 Karoumi Mouad. Tous droits réservés.
