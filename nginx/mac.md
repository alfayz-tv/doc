# install
```sh
brew install nginx

```

# start 
```sh
brew services start nginx

```

# trouble shot error start nginx
```sh
# unload launchctl
launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.nginx.plist   

# change chown pid nginx
cd /opt/homebrew/var/run
sudo chown <nama-user> nginx.pid

# then start again
```

