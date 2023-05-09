# Docker Compose
> example :
```sh
version: "3.8"
services:
    frontend:
        image: my-vue-app
        expose:
            - "80"
        ports:
            - "80:80"
        networks:
            - my-shared-network
        environment:
            DB: mydb
            USER: "alfa"
    backend:
        image: my-springboot-app
    db:
        image: postgres
volumes:
  ...
networks:
    my-shared-network: {}


```
# Building Image
```sh
# from dockerfile
services:
    my-custom-app:
        build: /path/to/dockerfile/

# from git
services: 
    my-custom-app:
        build: https://github.com/my-company/my-project.git
```