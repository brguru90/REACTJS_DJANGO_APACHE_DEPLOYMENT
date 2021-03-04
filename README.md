## REACTJS_DJANGO_APACHE_DEPLOYMENT STEPS:

### React production build
`cd reactappfolder` <br/>
`npm run build`

### Required file permissions:
`sudo chown www-data:www-data -R productionfolder/`

### Apache conf file is available in repo:
###### /etc/apache2/site-available/mynew.conf

```
<VirtualHost *:9876>
	#ServerName http://avpathak1-lnx.cisco.com:8000
	ServerName mysite.com
	ServerAlias localhost

	# SSLEngine on
	# SSLCertificateFile      /etc/ssl/certs/apache-selfsigned.crt
	# SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
	#SSLCACertificateFile /var/www/ssl/AAACertificateServices.crt

	# ProxyPreserveHost On
	# SSLProxyEngine On
	# SSLProxyCheckPeerCN on
	# SSLProxyCheckPeerExpire on

	# ProxyPass /app_fix http://127.0.0.1:8080/
	# ProxyPassReverse /app_fix http://127.0.0.1:8080/

	# ProxyPass /fix_server http://127.0.0.1:8080/fix_server
	# ProxyPassReverse /fix_server http://127.0.0.1:8080/fix_server

	LogLevel info
	WSGIScriptAlias /api /home/ko/djangoserver/apachereactdjango/djangoserver/djangoserver/wsgi.py
	WSGIDaemonProcess myprocessname processes=5 threads=10 display-name=%{GROUP} python-home=/home/ko/djangoserver/djangoser
	WSGIProcessGroup myprocessname
	WSGIApplicationGroup %{GLOBAL}
	
	#path to parent folder where both client and server dirs are present(this parent folder must have all necessary permissions required by apache)
	<Directory /home/ko/djangoserver/apachereactdjango>
		AllowOverride all
		Options +Indexes
		Require all granted
		Allow from all
	</Directory>
	
	
	ErrorLog /home/ko/djangoserver/error.log
	CustomLog /home/ko/djangoserver/access.log combined

	DirectoryIndex index.html
	# ErrorDocument 400 https://localhost:8888/
	# Static files location (_example: react build folder path)
	DocumentRoot /home/ko/djangoserver/apachereactdjango/build/


	# Django built static files (templates) folder
	# Alias /djangostatic /var/www/docker_installer/frontend/build/django/static 
	Protocols  h2 h2c  http/1.1

</VirtualHost>

```


## Ubuntu commands for apache and WSGI congiruration

### Install apache2 and WSGI in Ubuntu
`sudo apt-get install python3-pip apache2 libapache2-mod-wsgi-py3`

### Create a new apache conf file for your site
`sudo vim /etc/apache2/site-available/mynew.conf`

### Disable unused site ex: default site using:
`sudo a2dissite 000-default.conf`

### Check conf file syntax using:
`sudo apache2ctl configtest`  # it will return syntax OK, if there are no erros

### Enable required sites using:
`sudo a2ensite mynew.conf`

### Reload apache2 service using:
`sudo apache2ctl restart` or `sudo systemctl reload apache2`

## Run the below command to know the ips to access the site: 
`hostname -I`

### To map ip to hostname add entries in below file:
vim /etc/hosts

example:
cat /etc/hosts <br/>
`127.0.0.1	localhost` <br/>
`129.0.1.1	ko` <br/>
`131.0.1.1	mynewsite.com`

### To change port from default 80 to other modify ports.conf
`cat /etc/apache2/ports.conf`<br/>

###### If you just change the port or add more ports here, you will likely also
###### have to change the VirtualHost statement in
###### /etc/apache2/sites-enabled/000-default.conf
```
Listen 80

<IfModule ssl_module>
	Listen 443
</IfModule>

<IfModule mod_gnutls.c>
	Listen 443
</IfModule>
.
.
.
.
```
### In django's wsgi.py file append BASE_DIR(project_dir) to sys.path to avoid module import error:
```
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
sys.path.append(BASE_DIR)

```

