# fedosovdv_microservices
fedosovdv microservices repository

## ДЗ-12 (docker-2)

* Создание docker host на YC
```
yc compute instance create \
  --name docker-host \
  --zone ru-central1-a \
  --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 \
  --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1804-lts,size=15 \
  --ssh-key ~/.ssh/appuser.pub
docker-machine create \
  --driver generic \
  --generic-ip-address=<!!!_IP_ADDRESS_!!!> \
  --generic-ssh-user yc-user \
  --generic-ssh-key ~/.ssh/appuser \
  docker-host
eval $(docker-machine env docker-host)
```
* Создание образа

```
cd docker-monolith/

docker build -t reddit:latest .
```
* Пуш образа в Docker Hub

```
docker tag reddit:latest  <docker hub login>/otus-reddit:1.0
```

* Удаление docker host

```
docker-machine rm docker-host
yc compute instance delete docker-host
```
