Install Apache2
```
sudo apt-get install apache2
```
Create the file /etc/apt/sources.list.d/pgdg.list, and add a line for the repository

```
deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main
```

Import the repository signing key, and update the package lists
```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | \
  sudo apt-key add -
sudo apt-get update
```
