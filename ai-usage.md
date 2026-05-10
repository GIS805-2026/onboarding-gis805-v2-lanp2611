# Trace d'usage IA — GIS805

> Chaque interaction significative avec un outil IA doit être documentée ici.
> Ce fichier est **obligatoire** et évalué à chaque remise.

## Format par entrée

```
### YYYY-MM-DD — Séance SXX
- **Modèle :** (ChatGPT-4o, Claude, Copilot, etc.)
- **Prompt :** (copier-coller exact)
- **Résultat :** (résumé de ce que l'IA a produit)
- **Validation :** (comment vous avez vérifié/modifié le résultat)
- **Justification :** (pourquoi cette interaction était nécessaire)
```

<!-- Ajoutez vos entrées ci-dessous -->

### 2026-05-10 — Séance S00
- **Modèle :** GitHub Copilot Chat
- **Prompt :** « Consigne un résumé de nos échanges dans AI usage. Très concis SVP »
- **Résultat :** Résumé ajouté dans `ai-usage.md` indiquant que nous avons discuté de la question CEO S01, des données disponibles, du flux `make load`/`make check`, et du fait que les tables `dim_date` et `fact_sales` manquent dans DuckDB.
- **Validation :** J’ai vérifié que l’entrée apparaissait bien dans le fichier.
- **Justification :** Documenter l’usage de l’assistant pour garder une trace de la résolution de problème et des diagnostics de modèle.

### 2026-05-10 — Séance S01
- **Modèle :** GitHub Copilot Chat
- **Prompt :** « Inscrire la question du CEO dans S01 executive brief », « Inscrire une réponse exécutive ... », « Pour la décision de modélisation, indiquer que nous allons créer un schéma en étoile ... », « dans la section preuve, inscrire la requête SQL qui permet de générer ce schéma en étoile dans notre BD », « Ajouter une phrase indiquant que certains check on pass ... », « Dans la portion 'Décision de modélisation' du brief s01, ajouter des détails sur le grain, les mesures et les dimensions », « Donne moi 2 requêtes exploratoires que je peux exécuter en ligne de commande pour duckdb », « Répondre à la section validation dans S01 brief ... », « Refais la section en tenant compte de ces instructions ... », « Ajouter une phrase indiquant que certains check on pass ... »
- **Résultat :** `answers/S01_executive_brief.md` mis à jour avec la question CEO, la réponse exécutive, le grain, les mesures, les dimensions, le diagramme Merlin, le SQL de preuve et la validation basée sur des requêtes exploratoires DuckDB.
- **Validation :** J’ai relu le brief et confirmé que chaque section demandée était présente. J’ai aussi vérifié que les requêtes de validation utilisaient `raw_fact_sales` et retournaient des résultats non nuls.
- **Justification :** Tracer précisément les prompts qui ont guidé la construction du brief exécutif S01 et l’itération sur le modèle dimensionnel pour le cours.
