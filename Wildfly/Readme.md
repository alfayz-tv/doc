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

# install with homebrew
```sh

brew install wildfly-as
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

# add Datasource & drivers
## example add mariadb-java-client-3.4.1.jar

```sh
# ex:
# to directory wildfly
cd /opt/homebrew/Cellar/wildfly-as/wildfly-26.1.1.Final/bin

# run wildfly
./standalone.sh

# open new terminal
cd /opt/homebrew/Cellar/wildfly-as/wildfly-26.1.1.Final/bin

# install jar file
# go browser https://mvnrepository.com/artifact/org.mariadb.jdbc/mariadb-java-client/3.1.4

# add to this folder

# run jbos-cli
./jboss-cli.sh

#connect
connect

# Firstly, install the JDBC Driver as a Module:
module add --name=org.mariadb --resources=mariadb-java-client-3.1.4.jar --dependencies=javax.api,javax.transaction.api

# Next, register the Driver in the datasources subsystem:
/subsystem=datasources/jdbc-driver=mariadb:add(driver-name="mariadb",driver-module-name="org.mariadb",driver-class-name=org.mariadb.jdbc.Driver)

# Next, complete the Datasource installation as follows:
data-source add --jndi-name=java:/PANDICORE --name=PandiCoreDS --connection-url=jdbc:mariadb://localhost:3306/pandi_core --driver-name=mariadb --user-name=alfa --password=password
```