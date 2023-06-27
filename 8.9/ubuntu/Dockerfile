FROM ubuntu

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y wget unzip

RUN wget https://bstack-local-prod.s3.amazonaws.com/binaries/v8.9.3/BrowserStackLocal-linux-x64.zip && \
    unzip BrowserStackLocal-linux-x64.zip && \
    rm BrowserStackLocal-linux-x64.zip && \
    chmod +x /BrowserStackLocal

ENTRYPOINT [ "/BrowserStackLocal", "--source", "docker_8_9_ubuntu"]
