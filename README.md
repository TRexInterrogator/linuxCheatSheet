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

**Copy files from container**
```bash
docker cp mongodb:./backups/work01 ~/backups 
```

**Running docker behing ufw**
https://svenv.nl/unixandlinux/dockerufw/



# Docker sample with nginx

## Configure container(s)
You will need all containers to be on the same docker network in order to access the databse

```bash
docker network create webapps
docker network connect webapps containername
```

## Configure NGINX

Open `/etc/nginx/sites-available/default`

The config should look similar to this

```
location /sample/ {
    proxy_pass_header Server;
    proxy_set_header X-Read-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Scheme $scheme;
    proxy_set_header Host $http_host;
    proxy_set_header X-NginX-Proxy true;
    proxy_connect_timeout 5;
    proxy_read_timeout 240;
    proxy_intercept_errors on;

    proxy_pass http://localhost:5005/;
    proxy_redirect http://localhost:5005/ http://$host/sample/;
}
```
