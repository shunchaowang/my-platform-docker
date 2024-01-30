# my-docker-platform

- Run PostgreSQL,MySQL, MongoDB, and Redis in docker

- The project can used in development environment

- **Do not use in production environment**

## Prerequisites

- [docker](https://docs.docker.com/install/)
- [Git](https://git-scm.com/)

## Installing

git clone the project

```shell
git clone
```

## Running

```shell
cd my-docker-platform
docker compose up -d

# show docker-compose log
docker compose logs -f
```

## Databases username and password

| database | username | password |
| :------: | :------: | :------: |
|  mysql   |   root   |   root   |
|  mongo   |    /     |    /     |
|  redis   |    /     |    /     |
| postgres |  puser   |  ppass   |

## Docker

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

## License

This project is licensed under the MIT License - see the [LICENSE.md](https://github.com/techiall/docker-mysql-mongo-redis/blob/master/LICENSE) file for details.
