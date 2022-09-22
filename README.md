# sonarqube

SonarQube on Azure

On [DockerHub](https://hub.docker.com/r/beoski/sonarqube)


## Setup Postgres

```bash
docker network create sonarqube-network
docker volume create --name postgresql_data
docker run -d --name postgresql  --env ALLOW_EMPTY_PASSWORD=yes --env POSTGRES_USERNAME=sonar --env POSTGRES_PASSWORD=sonar --env POSTGRES_DATABASE=sonar --network sonarqube-network --volume postgresql_data:/var/lib/postgresql/data postgres:latest
```

Then connect to the running container and execute these commands:

```bash
su postgres
createuser sonar
psql postgres -W
ALTER USER sonar WITH ENCRYPTED password 'sonar';
CREATE DATABASE sonar WITH ENCODING 'UTF8' OWNER sonar;
```

## Setup SonarQube

```bash
docker volume create --name sonarqube_data
docker run -d --name sonarqube -p 80:9000 --env SONAR_JDBC_URL=jdbc:postgresql://postgresql:5432/sonar --env SONARQUBE_DATABASE_NAME=sonar --env ALLOW_EMPTY_PASSWORD=yes --env SONAR_DATABASE_USER=sonar --env SONAR_DATABASE_PASSWORD=sonar --network sonarqube-network --volume sonarqube_data:/opt/sonarqube/data beoski/sonarqube:9.6.1
```


