FROM ubuntu

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y wget unzip

RUN wget https://www.browserstack.com/browserstack-local/binaries/release/v8.8/BrowserStackLocal-linux-x64.zip && \
    unzip BrowserStackLocal-linux-x64.zip && \
    rm BrowserStackLocal-linux-x64.zip && \
    chmod +x /BrowserStackLocal

ENTRYPOINT [ "/BrowserStackLocal", "--source", "docker_8_8_ubuntu"]
