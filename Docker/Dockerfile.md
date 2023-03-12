# Build
> example config
```sh
# Install the base requirements for the app.
# This stage is to support development.
FROM --platform=$BUILDPLATFORM python:alpine AS base
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

FROM --platform=$BUILDPLATFORM node:18-alpine AS app-base
WORKDIR /app
COPY app/package.json app/yarn.lock ./
COPY app/spec ./spec
COPY app/src ./src

# Run tests to validate app
FROM app-base AS test
RUN yarn install
RUN yarn test

# Clear out the node_modules and create the zip
FROM app-base AS app-zip-creator
COPY --from=test /app/package.json /app/yarn.lock ./
COPY app/spec ./spec
COPY app/src ./src
RUN apk add zip && \
    zip -r /app.zip /app

# Dev-ready container - actual files will be mounted in
FROM --platform=$BUILDPLATFORM base AS dev
CMD ["mkdocs", "serve", "-a", "0.0.0.0:8000"]

# Do the actual build of the mkdocs site
FROM --platform=$BUILDPLATFORM base AS build
COPY . .
RUN mkdocs build

# Extract the static content from the build
# and use a nginx image to serve the content
FROM --platform=$TARGETPLATFORM nginx:alpine
COPY --from=app-zip-creator /app.zip /usr/share/nginx/html/assets/app.zip
COPY --from=build /app/site /usr/share/nginx/html

```
```sh
# build images 
# docker build -t namausername/namaimages:tag-images folder
docker build -t rofai/nginx-new:latest example
```

# build docker compose
```sh
# pindah ke folder yg ada file docker-compose.yaml
cd folder/
docker compose create

#list container yg di buat menggunakan docker compose
docker compose ps

#stop all semua container
docker compose stop

# delete container network and volume
docker compose down
```