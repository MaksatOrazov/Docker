# Hands-on Docker-04 : Networking 

## Pre-step: Creating EC2 Instance:
 
 * Create an EC2 Instance with the following ports and user data:

 * Ports:
 
 ```
 SSH 22 Anywhere
 HTTP 80 Anywhere
 Custom TCP 8080 Anywhere
 ```

 * User Data:
 
 ```bash
 #! /bin/bash

yum update -y
amazon-linux-extras install docker -y
systemctl start docker
systemctl enable docker
usermod -a -G docker ec2-user
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
 ``` 


## Goals of this training

At the end of the this hands-on practice, you will;

- bind containers to specific ports

- list available networks  

- create a network  

- inspect all properties of a network  

- connect a container to a network

- explain default network bridge configuration

- configure user-defined bridge network

- ping containers within same network

- delete networks
  

## Contents

- Part 1 - Port Binding
  
- Part 2 - Default Network Bridge  

- Part 3 - User-defined Network Bridge  




## Part 1 - Port Binding

- Run a `nginx` web server, name the container as `web`, and bind the web server to host port  80 command to run alpine shell. Explain `--rm` and `-p` flags and port binding.

```bash
docker run --rm -d -p  80:80 --name web nginx

- Stop container `web`, should be removed automatically due to `--rm` flag.

```bash
docker stop web
```




## Part 2 - Default Network Bridge  

- Check if the docker service is up and running.

```bash
systemctl status docker
```

- List all networks available  , and explain types of networks.

```bash
docker network ls
```

- Run two `alpine` containers with interactive shell, in detached mode, name the container as `techpro1st` and `techpro2nd`, and add command to run alpine shell. Here, explain what the detached mode means.

```bash
docker run -d -it --name techpro1st alpine sh
docker run -d -it --name techpro2nd alpine sh
```

- Show the list of running containers on Docker machine.

```bash
docker ps
```

- Show the details of `bridge` network, and explain properties (subnet, ips) and why containers are in the default network bridge.

```bash
docker network inspect bridge
```


- Connect to the `techpro1st` container.

```bash
docker attach techpro1st
```

- Ping `techpro2nd `container by its IP four times to show the connection.

```bash
ping -c 3 <techpro2ndIPv4> # 172.17.0.3
```

- Try to ping `techpro2nd `container by its name, should face with bad address. Explain why failed (due to default bridge configuration not works with container names)

```bash
ping -c 3 techpro2nd
```

- Disconnect from `techpro1st` with `exit`

- Stop and delete the containers

```bash
docker stop techpro1st techpro2nd
docker rm techpro1st techpro2nd
```

## Part 3 - User-Defined Bridge Network 

- Create a bridge network `techpronet`.

```bash
# Custom bridge Network olusturalim
docker network create --driver bridge techpronet
```

- List all networks available  , and show the user-defined `techpronet`.

```bash
docker network ls
```

- Show the details of `techpronet`, and show that there is no container yet.

```bash
docker network inspect techpronet
```

- Run four `alpine` containers with interactive shell, in detached mode, name the containers as `techpro1st`, `techpro2nd`, `techpro3rd` and `techpro4th`, and add command to run alpine shell. Here, 1st and 2nd containers should be in `techpronet`, 3rd container should be in default network bridge, 4th container should be in both `techpronet` and default network bridge.

```bash
docker run -d -it --network techpronet --name techpro1st alpine sh
docker run -d -it --network techpronet --name techpro2nd alpine sh

docker run -d -it --name techpro3rd alpine sh
docker run -d -it --name techpro4th alpine sh

docker network connect techpronet techpro4th
```

- List all running containers and show there up and running.

```bash
docker container ls
```

- Show the details of `techpronet`, and explore newly added containers. 


```bash
docker network inspect techpronet
# 1st, 2nd, and 4th containers should be in the list
```

- Show the details of  default network bridge, and explore newly added containers. 

```bash
docker network inspect bridge
# 3rd and 4th containers should be in the list
```

- Connect to the `techpro1st` container.

```bash
docker attach techpro1st
```

- Ping `techpro2nd` and `techpro4th` container by its name to show that in user-defined network, container names can be used in networking.

```bash
ping -c 3 techpro2nd
ping -c 3 techpro4th
```

- Try to ping `techpro3rd` container by its name and IP, should face with bad address because 3rd container is in different network.

```bash
ping -c 3 techpro3rd
ping -c 3 172.17.0.2
```

- Exit the `techpro1st` container with exit.
- Connect to the `techpro4th` container, since it is in both network should connect all containers.

```bash
docker attach techpro4th
```

- Stop and remove all containers

```bash
docker stop techpro1st techpro2nd techpro3rd techpro4th
docker rm techpro1st techpro2nd techpro3rd techpro4th
```

- Delete `techpronet` network

```bash
docker network rm techpronet
```