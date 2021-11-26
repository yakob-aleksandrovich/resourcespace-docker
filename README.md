# resourcespace-docker

[![Docker Pulls](https://img.shields.io/docker/pulls/joppes/resourcespace)](https://hub.docker.com/r/joppes/resourcespace)

A docker image for [ResourceSpace](https://www.resourcespace.com/svn) based on Ubuntu 20.04 LTS including OpenCV, poppler and php7.4. Older versions of ResourceSpace (9.2 and 9.3) are still based on 18.04 LTS and don't include poppler but xpdf. They also use php7.2.

Please report any issues on GitHub: https://github.com/joppes/resourcespace-docker/issues

Starting with ResourceSpace 9.4 the containers are going to be based on Ubuntu:latest (currently 20.04 LTS), as I tested poppler as a proper xpdf replacement - which isn't shipped in 20.04 anymore - in ResourceSpace.

## docker-compose example

The below docker compose file is a minimial working example.

```Dockerfile
version: "3.7"

networks:
  frontend:
  backend:

volumes:
  mariadb:
  include:
  filestore:

services:
  resourcespace:
    image: joppes/resourcespace:latest
    restart: unless-stopped
    depends_on:
      - mariadb
    volumes:
      - include:/var/www/html/include
      - filestore:/var/www/html/filestore
    networks:
      - frontend
      - backend
    ports:
      - 80:80

  mariadb:
    image: mariadb
    restart: unless-stopped
    env_file:
      - db.env
    volumes:
      - mariadb:/var/lib/mysql
    networks:
      - backend
```
