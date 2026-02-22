# 🏠 Analyse du Marché Immobilier - SQL
**Projet : Création et exploitation d'une base de données pour Laplace Immo**

---

## 📝 Contexte & Objectif
L'objectif de ce projet est de fournir des estimations de prix pertinentes en s'appuyant sur l'analyse des transactions immobilières du 1er semestre 2020. Cette approche permet à l'agence d'obtenir un avantage stratégique sur la concurrence grâce à la donnée.

---

## 🛠 Méthodologie & Stockage
Pour ce projet, j'ai mis en place un **SGBDR (PostgreSQL)** pour garantir :
* **La conformité RGPD** : Anonymisation des noms des acquéreurs.
* **La sauvegarde & Évolutivité** : Structure prête à accueillir les données des années suivantes pour affiner les modèles.

### Sources de données :
1. **Valeurs Foncières** (DGFIP) : Transactions du 1er semestre 2020.
2. **Référentiel Géographique** (INSEE) : Nomenclature des regroupements territoriaux.
3. **Données de Population** (INSEE) : Informations démographiques par commune.

---

## 📐 Modélisation (MLD)
Le schéma relationnel a été conçu pour lier efficacement les transactions aux données géographiques et démographiques.

<p align="center">  <img src="images/schema_relationnel.png" width="180" alt="Schéma relationnel"></p>

---

## 🔍 Analyses Clés (Extraits SQL)

Voici quelques-unes des 10 requêtes majeures réalisées pour répondre aux besoins métiers :

### 1. Les 10 départements les plus chers
*Question : Quels sont les départements où le prix moyen au m² est le plus élevé ?*

```sql
SELECT 
    d.nom_departement, 
    ROUND(AVG(t.valeur_fonciere / t.surface_reelle_bati), 2) AS prix_m2_moyen
FROM transactions t
JOIN communes c ON t.code_commune = c.code_commune
JOIN departements d ON c.code_departement = d.code_departement
WHERE t.surface_reelle_bati > 0
GROUP BY d.nom_departement
ORDER BY prix_m2_moyen DESC
LIMIT 10;
