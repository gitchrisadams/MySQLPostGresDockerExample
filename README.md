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