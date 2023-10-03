## Install Activemq
```sh
# mac
brew install activemq
```

## Start Activemq
```sh
brew services start activemq

# dashboard
http://127.0.0.1:8161/admin/ 
username: admin
password: admin
```

# Docker
```sh
# pull images
docker pull apache/activemq-artemis

# create container

# run 
docker run --rm -d --name activemq-artemis -p 61616:61616 -p 8161:8161 -p 1883:1883 apache/activemq-artemis:latest

```