## Build command
docker build -t devbase-supervisor .

## UP command
docker run -d --name devbase-supervisor devbase-supervisor

## LOGIN command
docker exec -it devbase-supervisor bash

## OTHER commands
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)

docker images -a | grep -vE "ubuntu" | awk '{print $3}' | xargs docker rmi

docker images -a | awk '{print $3}' | xargs docker rmi

docker rmi $(docker images -q) --force

