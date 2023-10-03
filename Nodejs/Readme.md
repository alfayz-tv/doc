# install node js with NVM
```sh
# download nvm
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash


# add to .bashrc
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion


# 
source .bashrc

# check version
nvm --version

# list 
nvm --list-rmote

#install node  ex: versi 16
nvm install  v16.18.1

# check versi node
node -v

# check versi npm
npm -v


# install yarn
npm install --global yarn

# check versi yarn
yarn -v
```

# pm2
```sh
# start golang project
pm2 start ./main --name main

# list process
pm2 list

# or
pm2 status

# look logs
pm2 logs main

#look service / process (main is name of process)
pm2 desc main 


# stop
pm2 stop main

# or you can stop all process
pm2 stop all

# delete all service
pm2 delete all
```

# Run service with ecosystem pm2

> example file ecosystem.config.js
```sh
module.exports = {
  apps : [
    {
      name: "registrar-api",
      cwd: '/var/www/html/registrar-api',
      script: "./main",
      args: "start",
      exec_interpreter: "none",
      exec_mode: "fork_mode",
      log_date_format: "YYYY-MM-DD HH:mm:ss Z",
      env: {
        APP_PORT: 3002
      }
    },
    {
      name: "registrar-admin",
      cwd: '/var/www/html/register-frontend-next/apps/admin',
      script: "node_modules/.bin/next",
      args: "start",
      exec_interpreter: "none",
      exec_mode: "fork_mode",
      log_date_format: "YYYY-MM-DD HH:mm:ss Z",
      env: {
        PORT: 3001
      }
    },
    {
      name: "registrar-user",
      cwd: '/var/www/html/register-frontend-next/apps/user',
      script: "node_modules/.bin/next",
      args: "start",
      exec_interpreter: "none",
      exec_mode: "fork_mode",
      log_date_format: "YYYY-MM-DD HH:mm:ss Z",
      env: {
        PORT: 3000
      }
    },
  ]
}


```

```sh
# and then run with pm2
pm2 start ecosystem.config.js

```

## update node version with nvm
```sh
# example with version node v18.18.0 
nvm install v18.18.0 

# list node version
nvm ls-remote

# install node
nvm install v18.18.0 

# set default node
nvm alias default X.X.X

# check node
node -v

```