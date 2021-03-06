# Getting Started
Spring Boot + Kotlin + MongoDB

## Requires:
```
1. `docker-compose`
2. `java 8 JDK` 
```

## Running Api + Mongo
```
# Run in project root
$ cd api
$ ./run.sh
```

## Running Api
```
# Run in project root
$ cd api
$ ./gradlew bootRun
```

## Efetuar autenticação para requisições
> Algumas requisições deverão ser autenticadas com usuário e senha.
*   *Usuário* = user
*   *Senha* = Buscar em logs para aplicação

> Container do docker.

$ docker container ls
```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                                                NAMES
40287cc3aa9b        fiap-api            "/bin/sh -c 'sh run.…"   About a minute ago   Up 59 seconds       0.0.0.0:5000->5000/tcp                               fiap-api
efc650fa0c5f        mongo:3.4           "docker-entrypoint.s…"   7 minutes ago        Up About a minute   0.0.0.0:12345->27017/tcp, 0.0.0.0:23456->28017/tcp   fiap-mongodb
```

> Buscar logs com CONTAINER ID. Exemplo: acima 40287cc3aa9b

$ docker container logs 40287cc3aa9b | grep -i "security password"
```
Using generated security password: 2babb967-9d13-4a7a-aed9-e9d972413304
```

> O usuário e senha deverão ser informados para algumas requisições onde a resposta seja 401:

Postman

<p align="center">
  <img src="https://raw.githubusercontent.com/WagnerCarvalho/fiap-spring/master/.github/postman.png?token=ABZNZYM6PQTDG6NEB4DEQTC6ST6F4" width="700">
</p>

Navegador
<p align="center">
  <img src="https://raw.githubusercontent.com/WagnerCarvalho/fiap-spring/master/.github/navegador.png?token=ABZNZYPSJUDKK3DXODDWOUS6ST6A6" width="700">
</p>

## Endpoints Ping Test
Requisição para Test de aplicação (Não é necessário Autenticar com usuário e senha) 
```
curl -X GET \
  http://localhost:5000/ping
```

## Endpoints Users
Criar Usuário (Não é necessário Autenticar com usuário e senha)
```
curl -X POST \
  http://localhost:5000/v1/fiap/create-user \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Wagner",
    "last_name": "Carvalho",
    "email": "wcarvalho@gmail.com",
    "doc": "323232323232",
    "password": "teste1",
    "birthday": "1990-02-29"
}'
```

Verificar Usuário (Necessário Autenticar com usuário e senha)
```
curl -X GET \
  http://localhost:5000/v1/fiap/get-user/{doc} \
  -H 'Authorization: Basic dXNlcjpiNzYwNDEwNC01OTc2LTQ3YzctOGY5OC0yZTIwYzgwZTg1NWI=' \
```

Atualizar dados do Usuário (Necessário Autenticar com usuário e senha)
```
curl -X PUT \
  http://localhost:5000/v1/fiap/update-user \
  -H 'Authorization: Basic dXNlcjphYmM2NzI0OC0zYzJlLTQ2MWUtYTdkMy1mNTg1YTNkNmIwMTU=' \
  -H 'Content-Type: application/json' \
  -d '{
	"id": "5e84bbac17a8846965394ce4",
	"name": "Jose"
}'
```

Deletar Usuário (Necessário Autenticar com usuário e senha)
```
curl -X DELETE \
  http://localhost:5000/v1/fiap/delete-user \
  -H 'Authorization: Basic dXNlcjo3MWMzODRhNS03ZmY5LTQ2YzktODBlMi0zNmZlNzIzMzJjMTU=' \
  -H 'Content-Type: application/json' \
  -d '{
    "id": "5e84bbac17a8846965394ce4"
}'
```


## Endpoints Transactions

Create Transactions
```
curl -X POST \
  http://localhost:5000/v1/fiap/create-transaction \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic dXNlcjoxZGIyMWM5Yy1iMDI3LTQ3ZmQtODgxMS1hZDhiODcwNTg1MmQ=' \
  -H 'Host: localhost:5000' \
  -d '{
  	"user_doc": "3095564",
  	"description": "Credito de alimento",
  	"amount": 10.0
  }'
```

Download PDF by Transactions
```
localhost:5000/v1/fiap/generate-extrato/${user_doc}
```

Login (Informar usuário e Senha)
<p align="center">
  <img src="https://raw.githubusercontent.com/WagnerCarvalho/fiap-spring/master/.github/extratoLogin.png?token=ABZNZYPPJQUOO6CCNN2MSWK6SUWHM" width="700">
</p>

PDF (Visualizar e fazer Download)
<p align="center">
  <img src="https://raw.githubusercontent.com/WagnerCarvalho/fiap-spring/master/.github/extrato.png?token=ABZNZYIVYETEZNRBAIVKQF26SUWEI" width="700">
