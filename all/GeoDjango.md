For the error:

permission denied to create extension "postgis"
HINT:  Must be superuser to create this extension.

Solution:

```
sudo -u postgres psql -d crimevis -c "CREATE EXTENSION IF NOT EXISTS postgis;"
```
