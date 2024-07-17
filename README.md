# TransIP DDNS

Create a docker container for updating a TransIP DNS record with the current public IP address.  
It monitors the current public IP address and when it changes it updates the DNS record hosted at TransIP.  
It does this every 4:30 to 5:00 minutes to make sure the DNS record is always up-to-date.  


# Using

This project uses:  
- [IPInfo.io](https://ipinfo.io/) API for getting the public IP address
- [transip/tipctl](https://github.com/transip/tipctl) TransIP Control the official TransIP RestAPI CLI for setting the IP address
- [php:cli-alpine](https://hub.docker.com/_/php/) Official php:cli-alpine docker image


## Usage

1. Put your private key in a file called `private.key` and store it next to the `docker-compose.yml` file.
1. Add the `tipddns` service to your `docker-compose.yml` file and change the `LOGINNAME` and `DOMAINS` environment variables.
1. Run `docker compose up -d` to start the container.

```yaml
services:
  tipddns:
    container_name: tipddns
    image: bvandevliet/tipddns:latest
    restart: unless-stopped
    volumes:
      - ./private.key:/app/private.key:ro
    environment:
      TZ: Europe/Amsterdam
      LOGINNAME: myusername
      DOMAINS: mydomain.com,myotherdomain.com
      ALWAYSLOG: true
```


# Environment variables

Variable   | Default value                  | Description
--         |--                              |--
LOGINNAME* | myusername                     | Your TransIP username.
DOMAINS*   | mydomain.com,myotherdomain.com | The domainnames registered at TransIP from which you want to update the A records.
ALWAYSLOG  | true                           | When set to true, log wil show IP has been checked but DNS did not need to be updated.
