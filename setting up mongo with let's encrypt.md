1- get server ip by running this commands :

`ip a | grep inet`


`sudo nano /etc/mongod.conf`

open mongo config and add server public ip to bindIp part to allow access from remote ips:
`bindIp: 127.0.0.1,<public ip>`



2- enable security in config

add this two lines to mongo config!

    security:
       authorization: enabled

3- create a user with full access and username password:

first open `mongosh` and

    db.createUser(
      {
      user: "hrmshams",
      pwd: passwordPrompt(),
      roles: [ { role: "root", db: "admin" } ]
      }
    )

4- add ssl created with let's letsencrypt

- first create a self signed certificate:

  Install let's encrypt ssl

  `sudo apt install python3-certbot-nginx`

  create cert

  `sudo certbot --nginx --agree-tos --no-eff-email --redirect -m <youremail@email.com> -d <sub.domainname.com>`

  Renewing SSL Certificate

  `sudo certbot renew --dry-run`

- combine fullchain and privatekey files and change its ownder

    `sudo cat /etc/letsencrypt/live/<domainname>/{fullchain.pem,privkey.pem} | sudo tee /etc/ssl/mongo.pem`

    `sudo chown mongodb:mongodb /etc/ssl/mongo.pem`

    `sudo chmod 600 /etc/ssl/mongo.pem`

- change mongodb config file

  `sudo nano /etc/mongod.conf`

  add following lines under net:

      net:
      ...
      #old
        ssl:
          mode: requireSSL
          PEMKeyFile: /etc/ssl/mongo.pem
      #new
        tls:
          mode: requireTLS
          certificateKeyFile: /etc/ssl/mongodb.pem

5- connect to mongo with this connection string :

    mongodb://<username>:<password>@<ipaddress>/?serverSelectionTimeoutMS=5000&tls=true&tlsAllowInvalidHostnames=true



source : https://zohaib.me/securing-mongodb-using-lets-encrypt/

notes :
  enabling firewall to allow mongo port traffic: `sudo ufw allow 27017/tcp`

other ways : https://www.digitalocean.com/community/tutorials/how-to-configure-remote-access-for-mongodb-on-ubuntu-20-04
