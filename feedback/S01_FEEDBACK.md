# Rétroaction automatisée -- S01 (Diagnostic fondamental -- NexaMart kickoff)

_Générée le 2026-05-14T22:27:44+00:00 -- Run `20260514T221333Z-7d34bf6a`_

Ce document est produit par un pipeline reproductible (vérification SQL déterministe + analyse LLM du brief et de la déclaration IA). Une revue humaine précède toujours sa publication. **À ce stade expérimental, aucune note ni étiquette de niveau n'est diffusée : l'objectif est purement formatif.**

---

## 1. Vérification automatique de la requête SQL

La requête extraite de votre brief n'a pas pu être validée automatiquement. Quelques pistes constructives ci-dessous pour vous aider à la rendre exécutable et alignee avec la question posée.

_Observation technique : erreur d'exécution SQL: Invalid Input Error: Cannot execute statement of type "CREATE" on database "cohort_reference" which is attached in read-only mode!_

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
> Votre `db/nexamart.duckdb` est absente ou vide ; la requête a été exécutée contre une **base de référence cohorte** (seed instructeur). Les chiffres retournés ne correspondent donc pas à vos propres données : reconstruisez votre base avec `python src/run_pipeline.py` (ou `.\run.ps1 load`) pour valider vos calculs sur votre seed personnel.
> Tables disponibles dans `db/nexamart.duckdb` : `raw_bridge_campaign_allocation`, `raw_bridge_customer_segment`, `raw_customer_changes`, `raw_customer_profile_bands`, `raw_customer_scd3_history`, `raw_dim_channel`, `raw_dim_customer`, `raw_dim_date`, `raw_dim_geography`, `raw_dim_product`, `raw_dim_segment_outrigger`, `raw_dim_store`, `raw_fact_budget`, `raw_fact_daily_inventory`, `raw_fact_inventory_snapshot`, `raw_fact_order_pipeline`, `raw_fact_orders_transaction`, `raw_fact_promo_exposure`, `raw_fact_returns`, `raw_fact_sales`.

## 2. Rétroaction pédagogique sur le brief

> Bon diagnostic technique et schéma en étoile correctement défini; le brief expose la nécessité de modéliser avant d'analyser. Renforcez la traçabilité (commits, note IA) et ajoutez des contrôles de validation reproductibles et des recommandations décisionnelles chiffrées.

### Observations par dimension

**Model quality**
- Observation : Le brief définit un schéma en étoile avec un grain « ligne de commande » (order_number, sale_line_id) et les dimensions date, catégorie, région et produit.
- Piste d'amélioration : Préciser le choix des patterns (ex. SCD type 2 sur dim_category) et documenter pourquoi certaines mesures sont non-additives (unit_price vs line_total).

**Validation quality**
- Observation : L'auteur fournit des CREATE TABLE DuckDB et deux requêtes COUNT / SUM sur raw_fact_sales pour vérifier la présence et les totaux des données sources.
- Piste d'amélioration : Ajouter des checks reproductibles (make check) ciblant les cas limites : NULLs, doublons du grain, et vérification que SUM(quantity * unit_price) = total attendu.

**Executive justification**
- Observation : La réponse exécutive indique clairement que les données actuelles ne permettent pas une réponse précise et recommande de modéliser les données avant d'analyser le déclin par catégorie et région.
- Piste d'amélioration : Formuler une recommandation opérationnelle chiffrée (p.ex. prioriser SCD sur dim_category et calendrier à livrer en X jours) pour guider la prise de décision du CEO.

**Process trace**
- Observation : Le brief mentionne que les données ont été chargées dans db/nexamart.duckdb via le pipeline, mais ne fournit pas d'historique de commits ni de note IA détaillée.
- Piste d'amélioration : Inclure un log de commits (≥3 commits avec messages), et une note IA précisant l'outil utilisé et la validation humaine effectuée.

**Reproducibility**
- Observation : Le DDL est fourni mais le brief référence un chemin local (`db/nexamart.duckdb`), ce qui indique des chemins codés en dur plutôt qu'un script de reproduction autonome.
- Piste d'amélioration : Fournir un script d'installation/chargement (« make load ») ou un README décrivant comment cloner, initialiser DuckDB et exécuter les checks sans chemins codés en dur.

## 3. Déclaration d'utilisation de l'IA

> La déclaration couvre bien les étapes d’utilisation, la validation humaine et les erreurs observées. Toutefois l'outil est nommé sans version/modèle précis, ce qui rend la partie «outils (version/modèle)» trop générique.

**Sujets bien couverts dans votre déclaration :**

- outils utilisés (nom + version/modèle)
- à quelle étape l'IA a été utilisée
- comment la sortie a été validée par l'humain
- limites ou erreurs observées

## 4. Pistes d'action pour la prochaine itération

- Reprendre la requête de la section « Preuve » pour qu'elle s'exécute sur `db/nexamart.duckdb` et qu'elle produise la forme attendue (voir pistes en section 1).

---

## 5. Traçabilité

- **Run ID :** `20260514T221333Z-7d34bf6a`
- **Devoir :** `S01`
- **Étudiant·e :** `lanp2611`
- **Commit analysé :** `fc542e2`
- **Audit (côté instructeur) :** `tools/instructor/feedback_pipeline/audit/20260514T221333Z-7d34bf6a/lanp2611/`
- **Prompts (SHA-256) :**
  - `sql_extractor_system` : `90ee9e277de7a27f...`
  - `rubric_grader_system` : `505f32d1d8319d66...`
  - `ai_usage_grader_system` : `81cb7fdf89bda55a...`
- **Fournisseur (rubrique) :** `openai`
- **Fournisseur (IA-usage) :** `openai` (gpt-5-mini-2025-08-07)

_Ce feedback a été produit par un pipeline automatisé et **revu par l'équipe pédagogique avant publication**. Aucun chiffre ni étiquette de niveau n'est diffusé à ce stade expérimental : l'objectif est uniquement formatif. Ouvrez une issue dans ce dépôt pour toute question._
