# Docker

- docker copy container data to host
```
docker cp $(docker-compose ps -q devtools):/tmp/folder .
```
