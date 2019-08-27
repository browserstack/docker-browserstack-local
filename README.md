# docker-browserstack-local
Docker official Image packaging for Browserstack Local Binary

### Installation
- Clone the repository
- Run ``` docker build  -t <build name:tag> <path to cloned folder>``` 

### Running the binary inside container with default command
 - Run the docker container using  
- ```docker run -it <build name:tag> --key <browserstack local testing access key>```
- The default entrypoint runs the binary without any arguments.

### Running the binary inside container with custom arguments
In order to run binary with custom [arguments](https://browserstack.com/local-testing) one can simply run the binary as follows.
`docker run <build name:tag> <arguments>`

Eg . Running binary inside container with force local and verbose 3 enabled
`docker run -it browserstack/local --key=<access key> --verbose 3 --force-local`

### Running the binary with a default custom set command
- Edit the Dockerfile and replace the content in ENTRYPOINT with whatever command or script you wish to execute. 
- Optionally you can also set the `BS_KEY` env variable inside the image itself. 
- After editing save the Dockerfile and build the docker image using the following command.
- Building the docker image
- `docker build -t <build: tagname> <path to dockerfile>`
- Now the image should be compatible to run the custom default command specified.

### Running tests for a server running on my network. 
Since the binary is running inside the docker container, it would try to resolve all the localhost requests internally. 

###Method 1
#### For Windows and Linux
- Run the docker container with the following flag `–net=host`. By setting this flag container’s network stack is not isolated from the Docker host. ( For more information refer here.)

- Running container with host netwroking
```docker run --net=host -it browserstack/local <arguments>```

#### For Mac
On Mac Docker runs its container on virtual machine. Henceforth, docker provides a way to access the host network using the url docker.for.mac.internal and for accessing localhost use `docker.for.mac.localhost`.

#### Method 2
Run the server on a public interface (eg. 0.0.0.0 ) and use the public address in your test scripts. 
  Disclaimer : PLEASE DO UNDERSTAND THE RISK OF RUNNING YOUR SERVER ON PUBLIC INTERFACE BEFORE FOLLOWING THIS METHOD ).

### FAQ's
* Can I run live and app-live using the binary running inside the container ?
&nbsp;&nbsp;&nbsp;&nbsp;No Live and App-live won't work if you are running BrowserStackLocal binary inside a Docker container.