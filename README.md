# Analyse des accidents corporels de la route en France (BAAC 2023)

Projet réalisé dans le cadre du cours **Python pour la Data Science** à l'ENSAI.

![Python](https://img.shields.io/badge/python-3.10+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Status](https://img.shields.io/badge/status-reproducible-brightgreen.svg)

---

## Sommaire

- [Problématique](#-problématique)
- [Données](#-données)
- [Arborescence du projet](#-arborescence-du-projet)
- [Installation et exécution](#-installation-et-exécution)
- [Contenu du notebook](#-contenu-du-notebook)
- [Principaux résultats](#-principaux-résultats)
- [Limites et pistes d'extension](#-limites-et-pistes-dextension)
- [Auteur](#-auteur)

---

## Problématique

> **Quels sont les principaux facteurs (temporels, environnementaux, humains et
> routiers) associés aux accidents corporels en France en 2023, et peut-on prédire
> la gravité d'un accident à partir de ces variables ?**

Quatre axes structurent le projet :

1. **Analyse descriptive** : identifier les dimensions qui expliquent la sinistralité.
2. **Cartographie** : localiser les zones à risque (heatmap, choroplèthe, accidents mortels).
3. **Analyse textuelle** : analyser ce que les français pensent de la sécurité routière en France
4. **Modélisation** : prédire si un accident sera *grave* (tué ou hospitalisé) à partir
   de variables disponibles au moment des faits.

---

## Données

Les données proviennent de la **base BAAC** (Bulletin d'Analyse des Accidents Corporels),
diffusée en open data sur [data.gouv.fr](https://www.data.gouv.fr/fr/datasets/bases-de-donnees-annuelles-des-accidents-corporels-de-la-circulation-routiere-annees-de-2005-a-2023/).

Le projet exploite les 4 fichiers de l'année **2023**, récupérés automatiquement via
l'API de data.gouv.fr :

| Fichier     | Granularité                  | Lignes (2023) | Colonnes|
|-------------|------------------------------|---------------|---------|
| `caract`    | un accident                  | 54 822        | 15      |
| `lieux`     | un lieu d'accident           | 70 860        | 18      |
| `usagers`   | une victime                  | 125 789       | 16      |
| `vehicules` | un véhicule impliqué         | 85 062        | 07      |


Les fichiers sont reliés entre eux par la clé **`Num_Acc`**.

Le fond de carte GeoJSON des départements est récupéré depuis le dépôt public
[france-geojson](https://github.com/gregoiredavid/france-geojson).

Pour l'analyse textuelle, les commentaires ont été collectés via l'**endpoint JSON public de Reddit** (`.json`
ajouté à l'URL d'un post), sans authentification ni clé API.

Les **9 posts** retenus traitent tous de sécurité routière , et proviennent de
trois subreddits : `r/france`, `r/voiture` et `r/EnculerLesVoitures`.
---

## Arborescence du projet

```
.
├── Analyse_accidents_BAAC_2023.ipynb   # Notebook principal (à exécuter)
├── README.md                            # Ce fichier
├── requirements.txt                     # Dépendances Python
└── outputs/                             # Généré à l'exécution
    ├── carte_heatmap.html
    ├── carte_choroplethe.html
    └── carte_mortels.html
```

---

## Installation et exécution

### 1. Cloner le dépôt

```bash
git clone https://github.com/Hermann19022002/Projet-final-Python-pour-la-data-science.git
cd Projet-final-Python-pour-la-data-science
```

### 2. Créer un environnement virtuel (recommandé)

```bash
python -m venv .venv
# macOS / Linux
source .venv/bin/activate
# Windows
.venv\Scripts\activate
```

### 3. Installer les dépendances

```bash
pip install -r requirements.txt
```

### 4. Lancer le notebook

```bash
jupyter notebook Analyse_accidents_BAAC_2023.ipynb
```

Puis exécuter toutes les cellules dans l'ordre (*Run All*). Toutes les données
sont téléchargées automatiquement — **aucun fichier local requis**.


> ```bash
> jupyter nbconvert --to notebook --execute Analyse_accidents_BAAC_2023.ipynb
> ```

---

##  Contenu du notebook

| Section | Objet |
|---|---|
| 1. Contexte & problématique | Cadrage du sujet |
| 2. Imports | Chargement des bibliothèques |
| 3. Récupération via API | Appel à l'API `data.gouv.fr` |
| 4. Nettoyage & enrichissement | Dates, âges, coordonnées, cible binaire |
| 5. Exploration statistique | Tableaux de synthèse, valeurs manquantes |
| 6. Analyses univariées | Temps, environnement, usagers, sécurité |
| 7. Analyses bivariées | Croisements avec la gravité |
| 8. Cartographie | Heatmap + choroplèthe + accidents mortels |
| 9. Modélisation | Régression logistique vs Random Forest |
| 10. Conclusion | Synthèse et limites |

Chaque graphique est accompagné d'un **encadré d'interprétation**.

---

##  Principaux résultats

- **17 %** des victimes sont **gravement blessées ou tuées** — classification déséquilibrée.
- La **nuit sans éclairage** multiplie la part d'accidents graves.
- Les **piétons** et les **plus de 65 ans** sont particulièrement vulnérables.
- Les **routes nationales et départementales** sont nettement plus mortelles que les
  voies urbaines, malgré un volume d'accidents moindre.
- Les accidents en volume sont concentrés en **Île-de-France**, mais les accidents
  **mortels** sont plus **ruraux**.
- Un **Random Forest équilibré** atteint une ROC-AUC d'environ **0.70** pour prédire
  la gravité, avec un recall bien supérieur à un modèle naïf.

---

## Limites et pistes d'extension

- Les variables **comportementales** (alcool, stupéfiants, vitesse, téléphone)
  absentes de la base BAAC limiteraient le pouvoir prédictif.
- L'analyse est **descriptive** : les associations mises en évidence ne sont pas
  causales.
- Pistes : optimisation d'hyperparamètres (GridSearch), XGBoost, dashboard
  Streamlit, croisement avec les données trafic (SIREDO) et météo (Météo-France).

---

##  Auteurs

Projet réalisé dans le cadre du cours *Python pour la Data Science* par  Hermann et Neville

Enseignant : **Lino Galiana** et **Raya Berova**

---

