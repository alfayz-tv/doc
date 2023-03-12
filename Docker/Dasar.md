# Images
```sh
# 1. list  images
docker images ls

# 2. pull images
docker images pull redis:latest

# 3. delete images
docker images rm redos:latest

```

# Container
```sh
# list all container
docker container ls -a

# list container runing
docker container ls

# create container
docker container create --name contohredisnama redis:latest

# start container
# example by id: docker container start container-id
# example by name: docker container start container-name

docker container start <contohredisnama>

# stop container
# can use container-id or container-name

docker container stop <contohredisnama>


# remove container
docker container rm <contohredisnama>

# container log
docker container logs <contohredisnama>

# container log with waiting debuging
docker container logs -f <contohredisnama>

# container Exec
# yaitu suatu perintah yang di gunakan untuk masuk ke container 
# biasanya di gunakan untuk menjalankan suatu perintah command line

docker container exec -i -t <contohredisnama> /bin/bash

# port forwading
# ex: docker container  create --name contohredisnama --publish porthost:portcontainer image-name:tag-versi

docker container create --name contohredisnama --publish 6379:6379 redis:latest

# environment variable
docker container create --name contohredisnama --env KEY="value" --env KEY2="value" redis:latest


# container stats
# untuk melihat resource yg di gunakan seperti memory, cpu dll
docker container stats


# resource limit
# kita bisa limit resource yg di gunakan pada container
# --memory , di gunakan untuk limit memory yg di gunakan
# --cpus , di gunakan untuk limit cpu yg di gunakan

docker container create smallnginx --memory 100m --cpus 1.5 --publish 8080:80 nginx:latest


# bind mounts
# sharing file / folder
# --mount ,confog : type, source, destination, readonly

docker container create mongodata --publish 27017:27017 --mount "type=bind,source=users/pandi-rofai/project/mongo-data,destination=/data/db" mongo:latest


# volumes
# sama seperti menggunakan binding yaitu menggunakan --mount
# dengan type=volume

docker container create mongovolume --mount "type=volume,source=namavolume,destionation=/data/db" mongo:latest


# network
# cara menggunakan network di container
# docker container create --name nginxpakenetwork --network namanetwork image:tag

# container 1
docker container --name mongocontainernama --network namanetwork --env ME_CONFIG_MONGODB_ADMINUSERNAME="root" --env ME_CONFIG_MONGODB_ADMINPASSWORD="example" mongo:latest

# container 2
docker container --name mongo-express --network namanetwork --env ME_CONFIG_MONGO_DB_URL="mongodb://root:example@mongocontainernama:27017/" mongo-express:latest

```

# Volumes
```sh
# list volume
docker volume ls

# create volume
docker volume create namavolume

#delete volume
docker volume rm namavolume

```

# Network
```sh
# list network
docker network ls

# create network
docker network create --driver bridge namanetwork

# delete network
docker network rm namanetwork

# hapus container dari network
# docker network disconnect namanetwork namacontainer
docker network disconnect namanetwork mongodb

# add container ke network
# docker network connect namanetwork namacontainer
docker network connect namanetwork mongodb
```

# inspect
> di gunakan untuk mengetahui lebih detail 
```sh
# inspect image
#docker image inspect namaimage
docker image inspect redis:latest

# inspect container
#docker container inspect namcontainer
docker container inspect exampleredis

# inspect volume
# docker volume inspect namavolume
docker volume inspect namavolume

# inspect network
# docker network inspect namanetwork
docker network inspect examplenetwork

```

# prune
> delete otomatis
```sh
# menghapus semua image yg sudah tidak di gunakan
docker image prune

# menghapus semua container yg sudah tidak di gunakan
docker container prune

# menghapus semua network yg sudah tidak di gunakan
docker network prune

#menghapus semua volume yg sudah tidak di gunakan
docker volume prune

```