## Build command
docker build -t facchin/devbase-bootstrap:alpine-3.10.3 .

## OTHER commands
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)

docker images -a | grep -vE "ubuntu" | awk '{print $3}' | xargs docker rmi

docker images -a | awk '{print $3}' | xargs docker rmi

docker rmi $(docker images -q) --force