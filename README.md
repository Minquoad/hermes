# Hermes

Notes d’exploitation pour l’environnement Hermes.

## Générer le mot de passe Caddy

```sh
docker exec reverse-proxy caddy hash-password --plaintext "..."
```

## Setup initial

Créez le Caddyfile à partir de l'exemple.

ajouter les droits/propriétaire :

```sh
chmod -R 777 storage-data
chown -R 10000:10000 storage-data

mkdir agent-data
chown 10000:10000 agent-data

mkdir webui-data
chown 10000:10000 webui-data
```

Lancer l'assistant de configuration Hermes :

```sh
docker run -it --rm \
  -e TZ=Europe/Paris \
  -e HERMES_TIMEZONE=Europe/Paris \
  -e OBSIDIAN_VAULT_PATH=/opt/storage/knowledges \
  -v ./agent-data:/opt/data \
  -v ./storage-data:/opt/storage \
  nousresearch/hermes-agent setup
```

Configuration attendue dans `config.yml` :

```yaml
terminal:
  cwd: /opt/storage
display:
  show_cost: true
```

Pendant la configuration, écraser les fichiers suivants si nécessaire :

- `MEMORY.md`
- `USER.md`
- `SOUL.md`

Si les messages vocaux ne sont pas gérés dans telegram :

```sh
docker exec -it hermes bash
uv pip install --python /opt/hermes/.venv/bin/python faster-whisper
/command/s6-svc -t /run/service/gateway-default
```

Dans l'UI web de silverbullet

- installer `treeview` (proposé nativement)
- installer `github:deepkn/silverbullet-graphview/graphview.plug.js`

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
