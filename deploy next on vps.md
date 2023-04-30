1- install "latest" nodejs

    curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
    sudo apt-get install -y nodejs

2- install pm2:
`sudo npm install pm2 -g`


3- ~ `git clone remoteurl` | `git pull`

4- ~ install dependencies : enter project folder and run `npm install`

5- build the project : `npm run build`


6- run the next built project via pm2:

  `pm2 start npm --name app1 -- run start -- -p 3000`

  make sure process is running :
  `pm2 list app1`


  other commands :

  `pm2 stop app1`

  `pm2 start app1`

  `pm2 restart app1`

  `pm2 delete app1`

  7- reverse proxy with ssl and nginx

------------------
shell file to re-deploy from a branch :

```bash
#!/bin/bash
# put production .env file inside ~ route
cp .env <project_address>/.env
cd <project_address>
sudo git pull origin <deploy branch>
git status
npm install
sudo npm run build
pm2 restart app1
```

sources :

1- https://episyche.com/blog/user-guide-for-deploying-the-nextjs-app-in-production-using-pm2-and-nginx

2- github CI/CD : https://codersteps.com/articles/how-i-integrated-a-simple-cicd-for-next-js-website-on-a-vps
