# DEVBASE

----------------------------------------------------
### PROPOSTA DO PROJETO
Disponibilizar imagens para facilitar o desenvolvimento de sistemas em distribuições linux

----------------------------------------------------
### Estrutura basica de desependencias de imagens
``` sh
# https://hub.docker.com/u/facchin

.
└── facchin/devbase-base:{tag}
    └── facchin/devbase-supervisor:{tag}
        └── facchin/devbase-bootstrap:{tag}
   

3 imagens uma dependente da outra, sendo a imagem 'devbase-base' utilizada
pra o Ambiente PHP
```
----------------------------------------------------
### Dockerfile "devbase-base"
``` sh
# Build image
FROM facchin/devbase-base:{tag} 
```

### Dockerfile "devbase-supervisor"
``` sh
# Build image
FROM facchin/devbase-supervisor:{tag} 
```

### Dockerfile "devbase-bootstrap"
``` sh
# Build image
FROM facchin/devbase-bootstrap:{tag} 
```

----------------------------------------------------
### Exemplo de uso
``` sh
$ cd app/basic-app/
$ docker-compose up -d
```