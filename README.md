# bookstack-qnapnas
Guideline and configuration for installing bookstack on QNAP NAS.


## 1, Install git tool on QNAP ssh shell

### Download entware qpkg file from link
https://www.qnapclub.eu/en/qpkg/556

### QNAP GUI
AppCenter -> "Install Manually" (right top button) -> Browser (downloaded entware qpkg file) then click Install

### QNAP NAS ssh shell
opkg update
opkg install git
opkg install git-http # for cloning repos via http instead of ssh


## 2, Prepare local storage for bookstack

### QNAP GUI
File Station 
  Create one "Shared Folder", for example a folder with name "bookstack"

Once shared folder is created, it could be seen under folder /share NAS ssh shell. So you could use this path to mount volume to docker container.



## 3, Clone bookstack repo

### QNAP NAS ssh shell
cd /share/bookstack
git clone https://github.com/BookStackApp/BookStack.git --branch release --single-branch
cd BookStack
mkdir node_modules

chown -R 1000:1000 node_modules #user node has uid 1000 in docker nodes:alpine
chmod 777 public/dist #to allow node generate files on the folder

## 4, update docker compose file

use docker compose file on this repo to update the cloned one

## 5, update .env file

copy .env file on this repo 

## 6, start bookstack docker compose

### QNAP NAS ssh shell
cd /share/bookstack/BookStack/
docker-compose up -d


## 6, generate key and migrate

### QNAP NAS ssh shell
docker exec -ti bookstack_app_1 sh

php artisan key:generate

php artisan migrate

 
