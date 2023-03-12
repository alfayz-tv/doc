#1. perintah ketika images di build

#instruction  FROM
FROM alpine:latest

# instruction run
RUN mkdir hello-folder
RUN echo "Hello World" > hello-folder/hello.txt
RUN cat hello-folder/hello.txt

#label instruction
LABEL author="rofai"
LABEL email="rofai@pandi.id"

# add instruction
ADD text/*.txt hello-folder

# copy instruction
COPY .env hello-folder/.env

# expose instruction
#Syntax: EXPOSE <port>/<protocol> 
EXPOSE 80/tcp
EXPOSE 80/udp

#environment variable
ENV nama Ahmad

#volume instruction
VOLUME /user/rofai/example-data

# working directory instruction
WORKDIR /project

# user instruction
RUN groupadd -r user && useradd -r -g user user
USER user

# argument instruction
ARG NAME=AlfaBet
RUN echo "Hey there! Welcome to $NAME" > greeting.txt

#2. perintah di jalankan ketika container jalan
# instuction CMD
# perintah ini akan di jalankan 1x jadi mohon untuk 1 perintah CMD saja
CMD cat hello-folder/hello.txt