# Install mongodb on ubuntu

## install mongo on ubuntu 20.04

> The first step is to install the prerequisite packages needed during the installation. To do so, run the following command.
```sh
sudo apt install -y software-properties-common gnupg apt-transport-https ca-certificates

```

> The official Ubuntu repositories provide the MongoDB package which can be installed in one command using the APT package manager as follows:
```sh
sudo apt install -y mongodb

```

> However, the version of MongoDB provided by the repositories is not the latest one. At the time of publishing this guide, the version provided by the Ubuntu repositories is v3.6.8. Meanwhile, the latest stable version provided by MongoDB is 5.0.


> To install the most recent MongoDB package, you need to add the MongoDB package repository to your sources list file on Ubuntu.



> But first, you need to import the public key for MongoDB on your system using the wget command as follows:

```sh
wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -

```
This command generates the following output indicating that the public key has been added

Ouput:
```sh
OK
```

Next, add MongoDB’s APT repository to the /etc/apt/sources.list.d directory.
```sh
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list
```

The command adds the mongodb-org-5.0.list file to the /etc/apt/sources.list.d/ directory. This file contains the following line:

```sh
deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse
```

Once the repository is added, reload the local package index.
```sh
sudo apt update
```

The command refreshes the local repositories and makes Ubuntu aware of the newly added MongoDB repository.

Once that is out of the way, install the mongodb-org meta-package that provides MongoDB.

```sh
sudo apt install -y mongodb-org
```