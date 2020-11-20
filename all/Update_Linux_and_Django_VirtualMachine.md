### Linux
Update and upgrade Ubuntu distribution:
```
sudo apt-get update; 
sudo apt-get upgrade;
sudo apt-get dist-upgrade;
sudo apt-get autoremove;
```
### Update Django
Django and its libraries must be installed and updated as **sudo** and normal user.
```
source /var/www/env/crimevis/bin/activate;
cd /var/www/app/django/crimevis;
sudo pip3 install --upgrade -r requirements.txt;
pip3 install --upgrade -r requirements.txt;
```
### Update the application
```
#!/bin/bash
source /var/www/env/crimewatcher/bin/activate
cd /var/www/app/spi-crime-watcher/
git fetch origin --verbose
git reset --hard origin/master
sudo rm /var/www/app/spi-crime-watcher/static_in_env/static_root/* -Rf
cd /var/www/app/spi-crime-watcher/crimevis
python3 manage.py collectstatic
python3 manage.py migrate
cd /var/www/
sudo chown www-data:www-data -R /var/www/app/
```
### Collect Static
```
source /var/www/env/crimevis/bin/activate;
cd /var/www/app/django/crimevis;
sudo rm /var/www/app/django/static_in_env/static_root/* -Rvf;
sudo python3 manage.py collectstatic;
```
