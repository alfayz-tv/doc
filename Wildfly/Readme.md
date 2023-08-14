# Wildfly

# install on mac (Manual)
```sh
# download
1. Go to http://wildfly.org/downloads/

# unzip

2. copy to folder 
# ex:  mv /Users/pandi-rofai/Downloads/wildfly-preview-23.0.0.Final /opt/homebrew/Cellar/wildfly-as   

3. start server
# cd 
/opt/homebrew/Cellar/wildfly-as/wildfly-preview-23.0.0.Final/bin
# start
./standalone.sh

# stop 
ctrl + x

# jika tidak bisa stop dengan ini
./jboss-cli.sh --connect command=:shutdown

# untuk restart
/jboss-cli.sh --connect command=:reload
```

# add user in wildfly
```sh
# to dir (on mac os)
cd /opt/homebrew/Cellar/wildfly-as/28.0.1/libexec/bin

# run add user
./add-user.sh -m -u guest -p guest
```

# then login
```sh
# url
http://localhost:9990

# username: guest
# password: guest

```