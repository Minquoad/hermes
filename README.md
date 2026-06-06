# Hermes

Notes d’exploitation pour l’environnement Hermes.

## Générer le mot de passe Caddy

```sh
docker exec reverse-proxy caddy hash-password --plaintext "..."
```

## Setup initial

Lancer l'assistant de configuration Hermes :

```sh
docker run -it --rm \
  -e TZ=Europe/Paris \
  -e HERMES_TIMEZONE=Europe/Paris \
  -e OBSIDIAN_VAULT_PATH=/opt/storage/knowledges \
  -v ./hermes-data:/opt/data \
  -v ./storage-data:/opt/storage \
  nousresearch/hermes-agent setup
```

Pendant la configuration, écraser les fichiers suivants si nécessaire :

- `MEMORY.md`
- `USER.md`
- `SOUL.md`

Configuration attendue dans `config.yml` :

```yaml
terminal:
  cwd: /opt/storage
display:
  show_cost: true
```

## Tâches à créer via le chat

Créer un skill pour compenser l'absence d'outil dans les crons :

- `cronjob`
- `messaging`

## Mise à jour

```sh
docker compose pull
docker compose down
docker volume ls
docker volume rm <nom du volume>
docker compose up -d && docker compose logs -f
```

## Commandes Hermes dans le conteneur

```sh
docker exec -it hermes hermes
```
