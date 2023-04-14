# how to backup and restore

## Backup

Ensure there is a /backups dir mounted in the docker compose setup

```
docker exec redmine-postgres-1 bash -c 'pg_dump -Fc redmine -U postgres | gzip > /backups/redmine_db-$(date +%Y-%m-%d).dump.gz'
```

## Restoring

From a fresh docker compose up:

1. Run only the postgres container:

```
docker compose up postgres -d
```

2. Restore from dump

```
gunzip redmine_db-*.dump.gz
```

```
docker exec -i redmine-postgres-1 pg_restore -U postgres -d redmine < ./backups/redmine_db-*.dump
```

3. Now you spin up the redmine container:
```
docker compose up redmine -d
```

4. Back up files
```
cd .. && tar -zcvf files-$(date +%Y-%m-%d).tar.gz files
```
