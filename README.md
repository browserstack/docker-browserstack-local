# docker-browserstack-local
Docker official Image packaging for Browserstack Local Binary

### Installation
- Clone the repository. 
- Depending on the choice of `binary_version` / `platform`, choose the appropriate Dockerfile 
- Run ``` docker build  -t <build name:tag> <path to dockerfile>``` 
- Eg. Build Image for Binary 8.5 on ubuntu platform.
- ```docker build -t local 8.5/ubuntu```

### Running the binary inside container with default command
 - Run the docker container using  
- ```docker run -it <build name:tag> --key <browserstack local testing access key>```
- The default entrypoint runs the binary without any arguments.
- For achieving best local testing experience. You may choose to bind binary services to host. 
-  `docker run -it -p 127.0.0.1:45691:45691 -p 127.0.0.1:45690:45690 -p 127.0.0.1:45454:45454 -p 127.0.0.1:45954:45954 <build name:tag> --key <access key>`

### Running the binary inside container with custom arguments
In order to run binary with custom [arguments](https://browserstack.com/local-testing) simply run the binary as follows.
`docker run -it <build name:tag> <arguments>`

Eg. Running binary inside container with force local and verbose 3 enabled
`docker run -it browserstack/local --key <access key> --verbose 3 --force-local`


### Running tests for a service running on the host. 
Since the binary is running inside the docker container, it would try to resolve all the host requests internally. Following methods can be used in order to overcome this limitation.

### Method 1
#### For Linux
- Run the docker container with the following flag `–net=host`. By setting this flag the container’s network stack is not isolated from the Docker host. ( For additional information refer [here](https://docs.docker.com/network/host/)).

- Running container with host netwroking
```docker run --net=host -it browserstack/local <arguments>```

#### For Mac and Windows
- Since Docker runs its container on a virtual machine on Mac and Windows. The method used in linux would only isolate the network interface between the virtual machine and the container. Hence, docker provides a way to access the host network using the url  `host.docker.internal`. (For additional information refer here ( [mac](https://docs.docker.com/docker-for-mac/networking/) | [windows](https://docs.docker.com/docker-for-windows/networking/) ).
- Additionally, the test scripts have to be modified to include the above url instead of localhost.
- Eg. If a service is running on port 3000, one can access the service by using host.docker.internal:3000.

#### Method 2
Run the server on a public interface (eg. 0.0.0.0 ) and use the public address in your test scripts. 
(Disclaimer : Please do understand the risk of running a service on public interface before following this method ).

### FAQ's
* Can I run live and app-live using the binary running inside the container?
   - Yes. For making live and app-live work, you need to bind 45691, 45690 ports with host.
   - Docker run command `docker run -it -p 127.0.0.1:45691:45691 -p 127.0.0.1:45690:45690 <build name:tag> --key <access key>`

* Can I access binary console dashboard from my host?
  - Yes. You need to bind docker 45454, 45954 ports with host.
  - Docker run command `docker run -it -p 127.0.0.1:45454:45454 -p 127.0.0.1:45954:45954 <build name:tag> --key <access key>`
