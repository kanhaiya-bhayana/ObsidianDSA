1. Enter to exec shell

```sh
$ docker exec -it <container-id> bash
```

2.  Enter the username
```sh
# psql -U <username>
```

now you will be connected to the user, and a terminal will be like below will be open for you:
![[pgsql-container-connection.png]]

###### Commands

1. To list the databases
```sh
\l
```

2. Connect to the database
```sh
\c <database-name>
```

3. Get the list of tables or relations in the database
```sh
\d
```

