# halo selfhost notes

Java + h2 embedded. Give the jvm a minute to warm up (~300mb ram).
Manual runs only.

## up

```bash
docker compose -f selfhost/docker-compose.yml up -d
docker logs -f halo   # wait for "Started Halo" line
```

open http://localhost:8086, create admin on first run.

## backup

h2 file — stop first or the copy is garbage:

```bash
docker compose -f selfhost/docker-compose.yml stop
docker run --rm -v halo-data:/data -v $(pwd)/backups:/b alpine \
  tar czf /b/halo-$(date +%F).tar.gz /data
docker compose -f selfhost/docker-compose.yml start
```

## postgres (if h2 starts hurting)

add a db service (postgres:16-alpine), switch the command flags to
`r2dbc:pool:postgres://...` + `--spring.sql.init.platform=postgresql`,
run once, migrate content via halo's built-in migration tool.
Not done by default — h2 is fine for a personal blog.

## update

bump tag, pull, up -d. Read the release notes first, halo minors
sometimes change required env.
