# Déploiement automatique sur Scaleway

## 1. Prérequis Scaleway
- Un compte Scaleway
- Un projet Scaleway
- Un Container Registry (ex: registry.fr-par.scw.cloud/mon-projet)
- Deux services Serverless Containers (un pour le backend, un pour le frontend)
- Une base de données managée (ex: PostgreSQL)

## 2. Secrets à ajouter dans GitHub
Dans les paramètres du repo GitHub > Settings > Secrets and variables > Actions, ajoute :
- `SCW_REGISTRY` : URL du registry (ex: registry.fr-par.scw.cloud/mon-projet)
- `SCW_REGISTRY_USER` : `_` (underscore)
- `SCW_REGISTRY_PASSWORD` : Token d'accès du registry (voir Scaleway > Container Registry > Credentials)
- `SCW_ACCESS_KEY` : Clé d'accès Scaleway
- `SCW_SECRET_KEY` : Secret Scaleway
- `SCW_PROJECT_ID` : ID du projet Scaleway
- `SCW_ORGANIZATION_ID` : ID de l'organisation Scaleway

## 3. Récupérer les IDs des containers
Après avoir créé tes services Serverless Containers sur Scaleway, récupère leurs IDs (ex: `11111111-aaaa-bbbb-cccc-222222222222`).
Remplace `<BACK_CONTAINER_ID>` et `<FRONT_CONTAINER_ID>` dans `.github/workflows/deploy.yml` par ces valeurs.

## 4. Variables d'environnement
Configure les variables d'environnement (ex: DATABASE_URL, JWT_SECRET, etc.) dans l'interface Scaleway pour chaque service container.

## 5. Pipeline CI/CD
À chaque push sur `main` :
- Build les images Docker
- Push sur le registry
- Déploie automatiquement sur Scaleway

## 6. Migration de la base de données
Si tu utilises Prisma, tu peux ajouter une étape dans le pipeline ou lancer la migration manuellement :
```
docker run --rm -e DATABASE_URL=... $BACK_IMAGE npx prisma migrate deploy
```

---

Pour toute question, consulte la [doc officielle Scaleway](https://www.scaleway.com/en/docs/). 