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