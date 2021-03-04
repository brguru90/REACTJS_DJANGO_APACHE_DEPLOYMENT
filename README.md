# REACTJS_DJANGO_APACHE_DEPLOYMENT STEPS:

# React production build
`cd reactappfolder`
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
>>cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	ko
127.0.1.1	mynewsite.com
