#### PostgreSQL Setup Using Docker Container
---
## Prerequisites
**Before you begin, you will need to have the following prerequisites:**

- A system running Windows, macOS, or Linux

- Docker installed on your system

- Basic knowledge of using the command line
**Once the docker is installed, you can verify the installation by running the following command in your terminal:**

```sh
$ docker --version
```
This command will display the version of Docker installed on your system.

- Now that you have Docker installed, create a new directory for your PostgreSQL data. This directory will be used to store the data files for your PostgreSQL instance. 

- I usually keep this directory in my project folder so that it is easy to manage.

Then, create a new file named docker-compose.yml in the same directory. This file will contain the configuration for your PostgreSQL container.

Insert the following content into the docker-compose.yml file:

```yml
version: '3'
services:
  postgres:
    image: postgres
    restart: always
    volumes:
      - ./data/db:/var/lib/postgresql/data
    ports:
      - 5432:5432  # make sure you don't have another container running on 5432

    environment:
      - POSTGRES_DB=TESTDB
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres

  pgadmin:
    image: dpage/pgadmin4
    restart: always
    ports:
      - 8080:8080
    environment:
      - PGADMIN_DEFAULT_EMAIL=user@domain.com
      - PGADMIN_DEFAULT_PASSWORD=localuser
```

Now that you have created the docker-compose.yml file, you can start the PostgreSQL container by running the following command in the terminal:

```sh
$ docker-compose up -d
```
