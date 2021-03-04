# REACTJS_DJANGO_APACHE_DEPLOYMENT STEPS:

# React production build
`cd reactappfolder` <br/>
`npm run build`

# Required file permissions:
`sudo chown www-data:www-data -R productionfolder/`

# Apache conf files are available repo


# Ubuntu commands for apache and WSGI congiruration
`sudo vim /etc/apache2/site-available/mynew.conf`

## Disable unused site like default site using:
`sudo a2dissite 000-default.conf`

## Check conf file syntax using:
`sudo apache2ctl configtest`  # it will return syntax OK, if there are no erros

## Enable required sites using:
`sudo a2ensite mynew.conf`

## Reload apache2 service using:
`sudo apache2ctl restart`

## Run the below command to know the ips to access the site: 
`hostname -I`

## To map ip to hostname add entries in below file:
vim /etc/hosts

example:
cat /etc/hosts <br/>
`127.0.0.1	localhost` <br/>
`129.0.1.1	ko` <br/>
`131.0.1.1	mynewsite.com`

## To change port from default 80 to other modify ports.conf
`cat /etc/apache2/ports.conf`<br/>

###### If you just change the port or add more ports here, you will likely also
###### have to change the VirtualHost statement in
###### /etc/apache2/sites-enabled/000-default.conf

Listen 80

<IfModule ssl_module>
	Listen 443
</IfModule>

<IfModule mod_gnutls.c>
	Listen 443
</IfModule>

#⋅⋅⋅vim: syntax=apache ts=4 sw=4 sts=4 sr noet


