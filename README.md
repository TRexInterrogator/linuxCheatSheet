# linuxCheatSheet


## QEMU

**Enlage virtual disk**
```bash
cd /var/lib/libvirt/images

sudo qemu-img resize disk.qcow2 +10G
```

## Docker

**Install Docker**
```bash
sudo apt install docker.io
sudo groupadd docker
sudo usermod -aG docker $USER
```

**Install mongodb**
```bash
docker run -d -p 27017-27019:27017-27019 --name mongodb mongo
```

**Docker networks**
```bash
docker network create mynetwork
docker network connect mynetwork container
```
**Backup and restore mongodb in container**
```bash
docker exec mongodb mongodump --archive --gzip --db mydb > mydb.zip 
docker exec mongodb mongorestore --archive --gzip --db=restoredb < mydb.zip
```

**Running docker behing ufw**
https://svenv.nl/unixandlinux/dockerufw/
