#### Ping from one machine to another on same network

```sh
$ docker run -it --network=<name> --name=<cont-name> <image>
```

```sh
root@ubuntu:/# apt update
```

```sh
root@ubuntu:/# apt install -y iputils-ping
```


```sh
root@ubuntu:/# ping <machine-name> 
```