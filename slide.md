---
layout: true

### Docker 1x1

---

## Was ist Docker?

Docker ist eine leichtgewichtige Alternative, die Containervirtualisierung unter Linux nutzt und so die Vorteile von virtuellen Maschinen ohne die Nachteile erreicht. 

### Dockerfile

Eine Textdatei, die Befehle enthält, um ein Image zu erstellen.

### Image 

Ein Image ist bei Docker eine portable Abbildung eines Containers.

### Container

Ein Container ist ein lauffähiges, virtuelles Betriebssystem.

---

## Docker Befehle

**Nützliche Docker Befehle:**

```bash
$ docker build - Baut ein Image auf Basis eines Dockerfiles
$ docker ps - Listet alle Container auf
$ docker logs - Gibt die Logs zu einem Container aus
$ docker pull - Lädt ein Image aus einer Registry
$ docker push - Speichert ein Image in einer Registry
$ docker run - Startet einen Container auf Basis eines Images
$ docker exec - Führt einen Befehl in einem laufenden Container aus
$ docker restart - Startet einen Container neu
$ docker images - Listet alle auf dem Host verfügbaren Images auf
$ docker help - Übersicht aller Befehle
```

[https://docs.docker.com/engine/reference/commandline/cli/](https://docs.docker.com/engine/reference/commandline/cli/)

---

## Der docker-compose Befehl

Mit dem `docker-compose` Befehl kann man definieren wie man Container starten will und wie diese Container in Beziehung stehen sollen. Des Weiteren kann man Verzeichnisse, Ports und Verlinkungen definieren. 

---

## Beispiel einer docker-compose.yml
 
```yml
version: "2"

services:
  nginx:
    image: nginx:1.13-alpine
    ports:
      - 80:80
    volumes_from:
      - php
    
  php:
    image: php:7.1-fpm-alpine              
    volumes:
      - "./Data:/data"
```

---

## Docker based development setup

Vorlage für eine auf Docker basierende [Entwicklungsumgebung](https://github.com/egobude/docker-project-template).

### Verwendung:

```bash
$ git clone https://github.com/egobude/docker-project-template.git
$ cd docker-project-template
$ cp .env.dist .env
$ docker-compose build
$ docker-compose up -d
```

Das Projekt erreicht ihr dann unter [http://<YOUR_IP_ADRESS:1234](http://<YOUR_IP_ADRESS:1234)

---

## Sicherung / Snapshot von einem Container erstellen

Wenn man mal einen Container sichern muss, kann man das wie folgt machen:

```bash
$ docker commit CONTAINER_ID_OR_NAME image-backup:$(date +%Y-%m-%d-%H-%M-%S)
```
        
---

## Backups

### Backup von Container-Volumes

Um Dateien oder ganze Ordner von laufenden oder gestoppten Containern zu sichern, kann man folgenden Befehl verwenden:

```bash
$ docker run --rm --volumes-from=my_mysql_container debian tar cvf - /var/lib/mysql | gzip > mysql-data.tar.gz
```

### MySQL Dump erstellen

```bash
docker run --rm --link=my_container:db --volumes=$(pwd)/backup:/backup mysql:latest mysqldump --host=db -u root my_database > /backup/my_database_dump.sql
```

---

## Dateien aus und in einen Container kopieren

Wenn ihr mal eine Datei aus einem Container benötigt oder einen Hotfix einspielen müsst, dann könnt ihr das wie folgt machen:

#### Dateien vom Container auf den Host kopieren 

```bash
docker cp CONTAINER:/datei/im/container /datei/auf/dem/host
```
    
#### Dateien vom Host in den Container kopieren

```bash
docker cp /datei/auf/dem/host CONTAINER:/datei/im/container
```

---

## Docker Image manuell auf einen Server kopieren

Manchmal kann es vorkommen, dass man von einem Docker-Host nicht auf die interne Registry zugreifen darf. Dann funktionieren solche Befehle wie `docker pull` oder `docker push` nicht mehr. Das lässt sich wie folgt umgehen:

##### Image als tar.gz Datei speichern

```bash
$ docker save busybox > busybox.tar.gz
```
   
##### tar.gz auf den Server übertragen

```bash
$ scp ./busybox.tar.gz root@IP_ADRESSE:/docker/busybox.tar.gz
```
    
##### tar.gz Datei laden und als Image ablegen 

```bash
$ docker load < /docker/busybox.tar.gz
```