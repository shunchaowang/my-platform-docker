# my-docker-platform

- Run PostgreSQL, MySQL, MongoDB, Redis and Kafka in docker

- The project can used in development environment

- **Do not use in production environment**

## Prerequisites

- [docker](https://docs.docker.com/install/)
- [Git](https://git-scm.com/)

## Installing

git clone the project

```shell
git clone https://github.com/shunchaowang/my-platform-docker
```

## Running

```shell
cd my-platform-docker
docker compose up -d

# show docker-compose log
docker compose logs -f

# show docker process
docker ps

# show all docker process
docker ps -a
```

## To get into the container shell

```shell
# list all running containers
docker ps

# use the container id from the ps
docker exec <container_id> -it bash
```

## Stopping

```shell
cd my-platform-docker
docker compose down
```

## Databases username and password

| database | username | password |
| :------: | :------: | :------: |
|  mysql   |   root   |   root   |
|  mongo   |    /     |    /     |
|  redis   |    /     |    /     |
| postgres |   user   |   pass   |

### Access mysql

I use DBeaver to connect to the mysql when docker is up

### Access mongo

I use mongo compass to access mongo when docker is up

### Access redis

TODO

### Access postgres

- use postgres sql command line to access the database
- use pdadmin dashboard to access the database

#### access the postgres using the cli on the host machine

once the docker compose is up, make sure the host has postgresql client installed, if not install it from the homebrew `brew install postgresql`

the port map is host:container, so 5432:5432 means we have local 5432 port mapped to the 5432 port in the container

```shell
# connect to the postgres db
psql -h localhost -p 5432 -U postgres
# put the password in the compose file when prompt
# list all dbs
\l
# exit
\q

```

#### access the postgres using the pdadmin ui in the container

open `http://localhost:5050` on the host browser, put in the admin email and password to login
Once login click the 'Add New Server', then under the `Connection` tab, put in the info below:
Host name/address: <get the ip by running `docker inspect postgres_container`>
Port: 5432
Username: postgres
Password: <password_from_the_compose_file>
finally 'Save

## Kafka

the docker container has 3 components, 3 kafka brokers, 1 zookeeper and 1 ui to access the system

### Brokers

all 3 brokers are configured the same way with different ports since they are running in the same container and host
the 3 brokers communicate using the advertised listeners configured
all brokers connect to the zookeeper on 2181

- kafka1 advertised listen on port 9092 internally and 29092 externally
- kafka2 advertised listen on port 9093 internally and 29093 externally
- kafka3 advertised listen on port 9094 internally and 29094 externally

### Zookeeper

zookeeper listens on 2181 and the port 2181 is mapped externally

### Kafka UI

kafka ui listens on 8080 internally and maps to 8085 externally and is accessible on 8085 from the host
the ui has a local cluster with bootstrap servers of the 3 kafka brokers
to access the ui once the docker container is up

```
curl http://localhost:8085
```

## Prometheus

Prometheus should be accessible at localhost with port 9090.

```curl http://localhost:9090

```

Note that the target for spring boot app should be using the real ip since prometheus runs in docker.

## Grafana

Grafana is a visualize tool for prometheus, once the platform runs, access localhost:3000, the default credential is admin/admin.

### Configure Prometheus on Grafana

Access grafana at localhost:3000, then add a new Dashboard by clicking 'Add Visualization', put premetheus as the data source, note to use the real ip.

## License

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/shunchaowang/my-platform-docker/LICENSE) file for details.
