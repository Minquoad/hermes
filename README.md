# Hermes

Notes d’exploitation pour l’environnement Hermes.

## Setup initial

Créez le Caddyfile à partir de l'exemple.

ajouter les droits/propriétaire :

```sh
mkdir storage-data
chown -R 10000:10000 storage-data

mkdir ../shared-data
chown -R 10000:10000 ../shared-data

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
  -e HERMES_WRITE_SAFE_ROOT=/opt/data:/tmp:/opt/storage:/opt/shared \
  -e OBSIDIAN_VAULT_PATH=/opt/storage/knowledges \
  -v ./agent-data:/opt/data \
  -v ./storage-data:/opt/storage \
  -v ../shared-data:/opt/shared \
  nousresearch/hermes-agent setup
```

Configuration attendue dans `config.yml` :

```yaml
terminal:
  cwd: /opt/storage
display:
  show_cost: true
stt:
  enabled: true
  provider: local
  local:
    model: small
    language: 'fr'
tts:
  provider: edge
  edge:
    voice: fr-FR-RemyMultilingualNeural
```

Pour un meilleure STT, remplacer `small` par `medium`.
Pour une TTS féminin, remplacer `fr-FR-RemyMultilingualNeural` par `fr-FR-VivienneMultilingualNeural`.

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

## Troubleshooting

### après un changement de clé d'api

Il faut manuellement mettre à jour cette clé dans `agent-data/.env`.

### après un changement de provider ou modèle

Il faut ensuite retirer les champs "provider_snapshot" et "model_snapshot" de agent-data/cron/jobs.json sinon les crons qui ont ces champs ne marche plus.
pour voir s'il y en a : `cat agent-data/cron/jobs.json | grep _snap`
