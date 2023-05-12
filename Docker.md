`docker rmi $(docker images -q)` - на остановленных контейнерах. удалит images
`docker network prune` - удалить сети
`docker inspect -f '{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq)` - узнать айпишники контейнеров
