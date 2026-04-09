# CLAUDE.md — Garmin Predictor

> Dernière mise à jour : 2026-04 — étape courante : **EDA** (`01_eda.ipynb`)

---

## Profil

| Attribut | Détail |
|----------|--------|
| Niveau Python | Confortable — fonctions, classes, pandas, notebooks |
| Niveau ML | Notebooks guidés / Kaggle |
| Contexte | Emploi plein temps → < 5h/semaine sur ce projet |
| Formation en cours | Data Science intensive (Python, ML, DL, LLM, RAG, Hugging Face, Docker, GCP) |
| Formation à venir | Data Analysis 5 mois (stats, SQL, Power BI, Tableau, Python, ML, Big Data) |
| Profil sportif | Athlète multi-sport : course à pied, hockey sur gazon, padel, hyrox, boxe |

---

## Projet

Application qui prédit chaque matin la catégorie d'effort adaptée à un athlète via ses données biométriques (comme une Garmin intelligente). Vocation multi-utilisateurs, sports personnalisables.

### Périmètre MVP vs post-MVP

| # | Problème | Statut |
|---|----------|--------|
| 1 | Prédire la catégorie d'effort journalière | **MVP** |
| 2 | Traduire la catégorie en activité concrète | Post-MVP — hors scope |

---

## Dataset

| Fichier | Description |
|---------|-------------|
| `data/garmin_like_daily_clean.csv` | 730 jours, 24 colonnes, 0 valeurs manquantes, données synthétiques, mono-individu (homme, 30 ans, 95–100 kg), 8 types d'activités dans `activity_type` |
| `data/garmin_like_data_dictionary.csv` | Dictionnaire complet — 55 entrées |

> `garmin_like_modeling_ready.csv` existe mais **ne sera pas utilisé** — le feature engineering sera fait manuellement à partir de `daily_clean`.

---

## Décisions prises

| Date | Décision | Justification |
|------|----------|---------------|
| 2026-04 | Classification 4 classes (pas régression) | Score continu = fausse précision sur données synthétiques |
| 2026-04 | Cible = seuillage de `readiness_score_today` | Évite tautologie avec `suggested_activity_today` |
| 2026-04 | Seuils provisoires : 0–40 Repos / 40–55 Léger / 55–70 Modéré / 70–100 Intense | Asymétrie voulue, à valider après distribution EDA |

---

## Questions ouvertes

- Les seuils seront-ils ajustés après observation de la distribution réelle du `readiness_score` ?
- Quelles features J-1 sont les plus prédictives ? → étape 4
- Critère d'arrêt MVP : à définir après baseline modèle → étape 5

---

## Pipeline

```
1. EDA                          ← ÉTAPE ACTUELLE
2. Feature engineering          ← shift J-1, rolling loads, encodages...
3. Définition du problème ML    ← régression vs classification, choix de la cible
4. Sélection des features       ← corrélations, importance, leakage check
5. Modélisation                 ← choix des modèles à faire
6. Évaluation                   ← choix des métriques à faire
7. Interprétabilité             ← SHAP (à découvrir)
8. Bridge données réelles       ← package garminconnect
```

---

## Règles de collaboration

### Ce que j'attends de Claude

| | Règle |
|-|-------|
| ✅ | Blocs actionnables de 30–90 min max — ne pas tout livrer d'un coup |
| ✅ | Code complet et fonctionnel, zéro pseudo-code |
| ✅ | Expliquer le pourquoi des choix techniques, pas la syntaxe de base |
| ✅ | Ton socratique : poser des questions pour me faire réfléchir avant de donner la réponse |
| ✅ | Toujours travailler depuis `garmin_like_daily_clean.csv` sauf instruction contraire explicite |
| ✅ | Signaler quand une décision structurante doit être prise — ne pas la prendre à ma place |
| ❌ | Pas d'architectures over-engineered inadaptées au temps disponible |
| ❌ | Pas d'explications sur `for`, `if`, `print`, pandas de base |
| ❌ | Ne pas anticiper les étapes futures tant que l'étape courante n'est pas terminée |

---

## Conventions de code

| Élément | Convention |
|---------|------------|
| Notebooks | Un par étape — `01_eda.ipynb`, `02_feature_engineering.ipynb`, etc. |
| Scripts utilitaires | Dans `src/` une fois les patterns stabilisés |
| Figures | `outputs/figures/` |
| Modèles | `outputs/models/` |
| Commentaires | Décisions uniquement — pas la syntaxe |

---

## Structure du projet

```
garmin-predictor/
├── data/
│   ├── garmin_like_daily_clean.csv
│   └── garmin_like_data_dictionary.csv
├── notebooks/
│   └── 01_eda.ipynb
├── src/
├── outputs/
│   ├── figures/
│   └── models/
├── CLAUDE.md
├── CONTEXT.md
└── requirements.txt
```

---

## End Goal features

- Connexion API réelle via `garminconnect` (package Python open-source)
- Multi-utilisateurs avec profils sportifs personnalisables
- Interface utilisateur

## Useful commands
```bash
# Install dependencies
pip install -r requirements.txt

# Launch Jupyter
jupyter notebook
```

## Stack

- **pandas** — data loading and manipulation - version 3.12.9
- **scikit-learn** — ML models and preprocessing
- **anthropic** — Claude API integration
- **jupyter** — interactive notebooks for exploration and analysis
