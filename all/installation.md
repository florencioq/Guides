## For both development and production
### Install Postgresql 10

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

Install the main package
```
sudo apt-get install postgresql-10
```
Change the password of the postgres user
```
sudo -u postgres psql
\password postgres
\q
```
Install pgadmin4 (this step is optional and it installs python 3.5)
```
sudo apt-get install pgadmin4
```
### Install python3.5
```
sudo apt-get install python3
```
Install virtualenv
```
sudo apt-get install virtualenv
```
### Install Geospatial Libraries and Postgis
```
sudo apt-get install binutils libproj-dev gdal-bin postgis
```
### Install Git
```
sudo apt-get install git
```
### Download of the master branch of the application
```
cd
mkdir clone
cd clone
git clone https://florencioq@bitbucket.org/florencioq/crimewatcher.git
```
### Create and activate the virtual environment
```
cd
mkdir env
virtualenv -p python3 env/crimewatcher
source env/crimewatcher/bin/activate
```
### Install Python libraries
```
cd
cd clone/crimewatcher/crimevis
pip3 install -r requirements.txt
```
### Create user crimevis
```
sudo -u postgres psql
create role crimevis password 'crimevis' login;
\q
```
### Change the login type for the user crimevis
```
Edit /etc/postgresql/10/main/pg_hba.conf and change:

    local   all             all                                     peer
to:
    local   all             all                                     md5

Then restart the server:

sudo service postgresql restart
```
### Create database crimevis
```
sudo -u postgres psql
create database crimevis owner crimevis;
\q
```
### Use pgadmin4 or pg_restore to restore a backup of the database crimevis
```
Try something like:
pg_restore --host "127.0.0.1" --port "5432" --username "postgres" -W --dbname "crimevis" "file.backup"
```
### Update the database, create a superuser and launch the system.
```
cd
source env/crimewatcher/bin/activate
cd clone/crimewatcher/crimevis
python3 manage.py migrate
python3 manage.py createsuperuser
python3 manage.py runserver
```
## For production
### Install Apache2 and wsgi
```
sudo apt-get install apache2
sudo apt-get install libapache2-mod-wsgi-py3 
```
### Installing the application in Apache subdirectories tree
```
cd /var/www
sudo mkdir app
cd app
sudo git clone https://florencioq@bitbucket.org/florencioq/crimewatcher.git
```
### Create the virtual environment
```
cd /var/www
sudo mkdir env
sudo virtualenv -p python3 env/crimewatcher
source env/crimewatcher/bin/activate
```
### Adjust permissions and reboot
```
sudo adduser $USER www-data
sudo chown www-data:www-data -R /var/www/env/    
sudo chmod -R 775 /var/www/env/
sudo chown www-data:www-data -R /var/www/app/    
sudo chmod -R 775 /var/www/app/
sudo reboot
```
### Install Python libraries
```
cd /var/www
source env/crimewatcher/bin/activate
cd /var/www/app/crimewatcher/crimevis
pip3 install -r requirements.txt
```
### Copy static files
```
cd /var/www
source env/crimewatcher/bin/activate
cd /var/www/app/crimewatcher/crimevis
python3 manage.py collectstatic
```
### Change /etc/apache2/sites-available/000-default.conf
```
<VirtualHost *:80>

	ServerAdmin webmaster@localhost
	DocumentRoot /var/www/html

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

	Alias /static /var/www/app/crimewatcher/static_in_env/static_root
	<Directory /var/www/app/crimewatcher/static_in_env/static_root>
   		Require all granted
 	</Directory>

	Alias /media /var/www/app/crimewatcher/static_in_env/media_root
	<Directory /var/www/app/crimewatcher/static_in_env/media_root>
   		Require all granted
	</Directory>

	<Directory /var/www/app/crimewatcher/crimevis/crimevis>
    		<Files wsgi.py>
        		Require all granted
    		</Files>
	</Directory>

	WSGIDaemonProcess crimevis python-home=/var/www/env/crimewatcher python-path=/var/www/app/crimewatcher/crimevis
	WSGIProcessGroup crimevis
	WSGIScriptAlias / /var/www/app/crimewatcher/crimevis/crimevis/wsgi.py

</VirtualHost>
```
### Reboot and everything should be in order
