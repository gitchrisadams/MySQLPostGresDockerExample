## Connecting to docker container to run MYSQL commands:
docker-compose exec CONTAINER_NAME_HERE mysql -root -pYOUR_PASSWORDHERE_NOSPACE_AFTER_THE_P DATABASE_NAME_HERE

## our recent mysql example:
docker-compose exec mysql-dev mysql -root -ppassword blogapp

## our legacy mysql example:
docker-compose exec mysql-legacy mysql -root -ppassword legacyapp

## Creating some sample data w/ sql insert:
```
CREATE TABLE `posts` (
  `id` int(10) NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `title` text NOT NULL
);
```

## Connecting to postgres:
docker-compose exec pgdb psql -U root -W blogapp

## SQL commands to create some sample data in postgres:
```
CREATE TABLE "posts" (
  "id" serial NOT NULL,
  "title" text NOT NULL
);
```

## Explanation of DockerCompose file:
```
# Specify DockerCompose version
version: '3'

# Must have a root level services
services:
  # Recent version of mysql
  # This is the host/service name.
  # Set environment to your own password.
  mysql-dev:
    image: mysql:8.0.0
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: blogapp
    # Maps yourlocalcomputer:dockerContainer
    # IN this case 3306 in docker gets mapped to 3308 on your local computer.
    ports:
      - "3308:3306"
    # Volume mounts local:docker
    # In this case we are mapping the local .my.conf to the .cnf file in container.
    # We are also mapping the mysql folder in read/write mode in docker to the
    # data folder on the host pc.
    volumes:
      - ".my.conf:/etc/mysql/conf.d/config-file.cnf"
      - "./data:/var/lib/mysql:rw"

  # Legacy version of mysql.
  # All these are the same as above
  # We are just using a mysql:5.7 image from docker hub.
  mysql-legacy:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: legacyapp
    ports:
      - "3309:3306"
  
  # PostGres image/container, same as above.
  pgdb:
    image: postgres
    environment:
      POSTGRES_USER: root
      POSTGRES_PASSWORD: password
      POSTGRES_DB: blogapp

  # For managing database docker containers:
  # This allows us to go to localhost:8080 and use a gui
  # to manage our db.
  admin:
    image: adminer
    ports:
      - 8080:8080
```