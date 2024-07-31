
What is Docker and why it is used?

Docker is **a software platform that allows you to build, test, and deploy applications quickly**. Docker packages software into standardized units called containers that have everything the software needs to run including libraries, system tools, code, and runtime.
#### Daily Needs Commands
	$ docker version
	
	$ docker ps
			-> list the running containers
			
	$ docker rm <id/name> <id/name> <id/name>
			-> to remove multiple containers, but this removes only containers that are stopped
			
	$ docker run -d -p 8800:80 httpd

	$ ps aux
			-> information about running processes
			
	$ docker top <container_name/id>
			-> Display the running processes of a container
			
	$ docker logs <container_name/id>
			-> to log processes running inside the container
			
	$ docker image ls
			-> list the images you have
			
	$ docker --help
			-> to get a list of commands we can use
			
	$ docker inspect <container_name>
			->show metadata about the container (startup config, volumes, networking, etc)
	
	$ docker stats 
			-> performance stats for all containers
	
			

$ docker top <container-name/id> # display the running processes of a container

The default output renders all version information divided into two sections; the `Client` section contains information about the Docker CLI and client components, and the `Server` section contains information about the Docker Engine and components used by the Docker Engine, such as the containerd and runc OCI Runtimes.

For more: https://docs.docker.com/reference/cli/docker/version/

The information shown may differ depending on how you installed Docker and what components are in use.

### [Client and server versions](https://docs.docker.com/reference/cli/docker/version/#client-and-server-versions)

Docker uses a client/server architecture, which allows you to use the Docker CLI on your local machine to control a Docker Engine running on a remote machine, which can be (for example) a machine running in the cloud or inside a virtual machine.

## What is the cURL command?

[Client URL](https://curl.se/) (cURL, pronounced “curl”) is a command line tool that enables data exchange between a device and a server through a terminal. Using this command line interface (CLI), a user specifies a server URL (the location where they want to send a request) and the data they want to send to that server URL.


#### Image vs. Container
- An image is the application we want to run
- A container is an instance of that image running as a process
- You can have many contaienrs running off the same image

#### What happens in 'docker container run'
1. Looks for that image locally in image cache, doesn't find anything
2. Thne looks in remote image repository (defaults to Docker Hub)
3. Downloads the latest version (nginx: latest by default)
4. Creates new container based on that image and prepares to start
5. Gives it a virtual IP on a private n/w inside docker engine
6. Opens up port 80 on host and forwards to port 80 in container
7. Starts container by using the CMD in the image Dockerfile


![[Pasted image 20240223121020.png]]

#### Containers vs. VMs
| Containers aren't Mini-VM's
- They are just processes
- Limited to what resources they can access
- Exit when process stops



## What's Going On In Containers | CLI Process Monitoring

```sh
$ docker container top <containerName>
```
	 Display the running processes of a container

```sh
$ docker container inspect 
```
		Show metadata about the container (startup config, volumes, networking, etc)

```sh
$ docker container stats
```
		Performance stats for all containers




## Getting a Shell inside Containers | No Need for SSH

```sh
$ docker container run -it
```
		Starts new container interactively

```sh
$ docker container exec -it
```
		Run additional command in existing container

> -it 
> -i :  interactive
> 	keep session open to receive terminal input
> -t :  pseudo-tty
> 	simulates a real terminal, like what SSH does
> bash shell :  if run with -it, it will give you a terminal inside the running container

### Different Linux distros in containers

```sh
$ docker container run -it --name proxy nginx bash
```

```sh
$ docker container exec -it mysql bash
```

nginx root file path: /_usr/share/nginx/html_.

## Docker Networks: Concept
- Each container connected to a private virtual n/w "bridge"
- Each virtual n/w routes through NAT firewall on host IP
- All containers on a virtual n/w can talk to each other without -p
- Best practice is to create a new virtual n/w for each app:
	- network "my_web_app" for mysql and php/apache containers
	- network "my_api" for mongo and nodejs containers

```sh
$ docker container run -p
```
		For local dev/testing, networks usually "just work"

```sh
$ docker container port <container-name>
```
		Quick port check with

```sh
$ docker insepct --format {{.NetworkSettings.IPAddress}} <contianer-name>
```
		Return the ip address


## Docker Networks : CLI Management

1. Show networks
```sh
$ docker network ls
```

2. Inspect a network 
```sh
$ docker network inspect <network-name>
```

3. Create a network
```sh
$ docker network create --driver
```

4. Attach a network to container
```sh
$ docker network connect
```

5. Detach a network from container
```sh
$ docker network disconnect
```


### Docker Networks : Default Security
- Create your apps so frontend/backend sit on same Docker network
- Their inter-communication never leaves host
- All externally exposed ports closed by default
- You must manually expose via -p, which is better default security!
- This gets even better later with Swarm and Overlay networks


## Docker Networks : DNS and How Containers Find Each Other

#### Docker DNS
- Docker daemon has a built-in DNS server that containers use by default


### 3 ways to build Docker Images

Dockerfile
Paketo - BuldPacks
Google JIB



