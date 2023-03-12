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

```