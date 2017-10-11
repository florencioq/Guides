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
sudo pip3 install --upgrade -r requirements.txt
pip3 install --upgrade -r requirements.txt
```
### Update the application
```
source /var/www/env/crimevis/bin/activate;
cd /var/www/app/django;
sudo git fetch origin;
sudo git reset --hard origin/master
```
### Collect Static
```
source /var/www/env/crimevis/bin/activate;
cd /var/www/app/django/crimevis;
sudo rm /var/www/app/django/static_in_env/static_root/* -Rvf;
sudo python3 manage.py collectstatic;
```