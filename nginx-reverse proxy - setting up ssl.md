# how to set up SSL with Nginx

1- install nginx:

`sudo apt install nginx`

2- remove default nginx configs:

`sudo rm /etc/nginx/sites-available/default`

`sudo rm /etc/nginx/sites-enabled/default`

3- create a new nginx configs:

`sudo nano /etc/nginx/sites-available/search.conf`

put this into file:

     server {
       listen [::]:80;
       listen 80;
       server_name sub.yourdomain.com;
     location / {
       proxy_pass http://localhost:port;
       proxy_redirect off;
       proxy_read_timeout    90;
       proxy_connect_timeout 90;
       proxy_set_header  X-Real-IP  $remote_addr;
       proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
       proxy_set_header  Host $http_host;
     }
    }


4- Enable your configuration by creating a symbolic link.

`sudo ln -s /etc/nginx/sites-available/search.conf /etc/nginx/sites-enabled/search.conf`


5- Install let's encrypt ssl:

`sudo apt install python3-certbot-nginx`

6- create cert

`sudo certbot --nginx --agree-tos --no-eff-email --redirect -m <youremail@email.com> -d <sub.domainname.com>`

7- Renewing SSL Certificate

`sudo certbot renew --dry-run`


8- Renewing SSL Certificate

`sudo systemctl reload nginx`

source:
<https://www.cloudbooklet.com/how-to-install-elasticsearch-on-ubuntu-22-04-with-ssl/>
