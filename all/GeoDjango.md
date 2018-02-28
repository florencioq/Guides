For entering in the psql:
```
sudo -u postgres psql
```

For the error:

permission denied to create extension "postgis"
HINT:  Must be superuser to create this extension.

Solution:

```
sudo -u postgres psql -d crimevis -c "CREATE EXTENSION IF NOT EXISTS postgis;"
```
For dropping a database:

a) drop connections:
```
SELECT
 pg_terminate_backend (pg_stat_activity.pid)
FROM
 pg_stat_activity
WHERE
 pg_stat_activity.datname = 'target_database';
```
 
b) drop the database:
```
 drop database paper;
```
