# Rétroaction automatisée -- S01 (Diagnostic fondamental -- NexaMart kickoff)

_Générée le 2026-05-15T12:40:52+00:00 -- Run `20260515T122624Z-00a5a04f`_

Ce document est produit par un pipeline reproductible (vérification SQL déterministe + analyse LLM du brief et de la déclaration IA). Une revue humaine précède toujours sa publication. **À ce stade expérimental, aucune note ni étiquette de niveau n'est diffusée : l'objectif est purement formatif.**

> ⚠️ **Avertissement instructeur (à retirer avant publication) :** cette analyse a été générée avec `--skip-pull`. Le contenu correspond au commit local et **n'est peut-être pas la dernière version poussée par l'étudiant·e**.

---

## 1. Vérification automatique de la requête SQL

La requête extraite de votre brief n'a pas pu être validée automatiquement. Quelques pistes constructives ci-dessous pour vous aider à la rendre exécutable et alignee avec la question posée.

_Observation technique : erreur d'exécution SQL: Invalid Input Error: Cannot execute statement of type "CREATE" on database "5e91435" which is attached in read-only mode!_

<details><summary>Requête analysée — cliquez pour déplier</summary>

```sql
CREATE TABLE dim_date (
    date_key    INTEGER PRIMARY KEY,
    date        DATE,
    year        INTEGER,
    quarter     TEXT,
    month_name  TEXT
);

CREATE TABLE dim_category (
    category_id INTEGER PRIMARY KEY,
    category    TEXT
);

CREATE TABLE dim_region (
    region_id   INTEGER PRIMARY KEY,
    region      TEXT
);

CREATE TABLE dim_product (
    product_id   INTEGER PRIMARY KEY,
    product_name TEXT,
    category_id  INTEGER REFERENCES dim_category(category_id)
);

CREATE TABLE fact_sales (
    sale_line_id INTEGER PRIMARY KEY,
    order_number TEXT,
    order_date   INTEGER REFERENCES dim_date(date_key),
    product_id   INTEGER REFERENCES dim_product(product_id),
    category_id  INTEGER REFERENCES dim_category(category_id),
    region_id    INTEGER REFERENCES dim_region(region_id),
    quantity     INTEGER,
    amount       DECIMAL
);
```

</details>


**Pistes :**
> Tables disponibles dans `db/nexamart.duckdb` : `raw_bridge_campaign_allocation`, `raw_bridge_customer_segment`, `raw_customer_changes`, `raw_customer_profile_bands`, `raw_customer_scd3_history`, `raw_dim_channel`, `raw_dim_customer`, `raw_dim_date`, `raw_dim_geography`, `raw_dim_product`, `raw_dim_segment_outrigger`, `raw_dim_store`, `raw_fact_budget`, `raw_fact_daily_inventory`, `raw_fact_inventory_snapshot`, `raw_fact_order_pipeline`, `raw_fact_orders_transaction`, `raw_fact_promo_exposure`, `raw_fact_returns`, `raw_fact_sales`.

## 2. Rétroaction pédagogique sur le brief

> Le brief présente un schéma en étoile cohérent avec un grain explicite et des DDL plausibles ; la justification métier est claire et la direction recommandée est pertinente. Il manque néanmoins des contrôles de validation automatisés, une trace de développement (commits/IA) et des checks reproductibles pour mettre en production rapidement.

### Observations par dimension

**Model quality**
- Observation : « Grain : fact_sales au niveau ligne de commande, identifié par (order_number, sale_line_id). » — le grain est explicite et un schéma en étoile est proposé.
- Piste d'amélioration : Préciser le pattern SCD choisi (ex. SCD Type 2) et expliciter le traitement des attributs non-additifs (unit_price vs line_total) avec exemples de requêtes.

**Validation quality**
- Observation : L’étudiant fournit des CREATE TABLE et des requêtes DuckDB montrant des contrôles de comptage et d’agrégation sur raw_fact_sales.
- Piste d'amélioration : Ajouter des contrôles automatisés (make check) couvrant NULLs, doublons du grain, et cas limites (SUM(quantity*unit_price), valeurs manquantes) et montrer les résultats attendus/observés.

**Executive justification**
- Observation : La réponse exécutive indique que « les données actuelles ne sont pas structurées pour répondre à cette question stratégique » et recommande la modélisation avant analyse.
- Piste d'amélioration : Formuler une recommandation concise au CEO avec un impact chiffré attendu (p.ex. délai et métrique de qualité des rapports) pour faciliter la décision.

**Process trace**
- Observation : Aucun historique de commits ni note sur l’usage d’IA ou journal de décision n’est fourni dans le brief.
- Piste d'amélioration : Inclure un petit log de commits (≥3) et une note IA décrivant outils utilisés, prompts et validation humaine.

**Reproducibility**
- Observation : Le brief mentionne le fichier db/nexamart.duckdb et des commandes duckdb, mais pas de script reproductible ni README détaillé.
- Piste d'amélioration : Ajouter un README pas-à-pas et un script check.sh qui, cloné dans le repo, exécute la création/chargement et les vérifications sans ajustements manuels.

## 3. Déclaration d'utilisation de l'IA

> La déclaration est complète sur les étapes d'utilisation, la validation humaine et les erreurs observées. Toutefois, le renseignement de l'outil reste générique (absence de version/modèle précis), d'où une note réduite.

**Sujets bien couverts dans votre déclaration :**

- outils utilisés (nom + version/modèle)
- à quelle étape l'IA a été utilisée
- comment la sortie a été validée par l'humain
- limites ou erreurs observées

## 4. Pistes d'action pour la prochaine itération

- Reprendre la requête de la section « Preuve » pour qu'elle s'exécute sur `db/nexamart.duckdb` et qu'elle produise la forme attendue (voir pistes en section 1).

---

## 5. Traçabilité

- **Run ID :** `20260515T122624Z-00a5a04f`
- **Devoir :** `S01`
- **Étudiant·e :** `lanp2611`
- **Commit analysé :** `5e91435`
- **Audit (côté instructeur) :** `tools/instructor/feedback_pipeline/audit/20260515T122624Z-00a5a04f/lanp2611/`
- **Prompts (SHA-256) :**
  - `sql_extractor_system` : `90ee9e277de7a27f...`
  - `rubric_grader_system` : `505f32d1d8319d66...`
  - `ai_usage_grader_system` : `81cb7fdf89bda55a...`
- **Fournisseur (rubrique) :** `openai`
- **Fournisseur (IA-usage) :** `openai` (gpt-5-mini-2025-08-07)

_Ce feedback a été produit par un pipeline automatisé et **revu par l'équipe pédagogique avant publication**. Aucun chiffre ni étiquette de niveau n'est diffusé à ce stade expérimental : l'objectif est uniquement formatif. Ouvrez une issue dans ce dépôt pour toute question._
