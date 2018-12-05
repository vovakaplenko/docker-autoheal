# docker-autoheal

This is a fork of https://github.com/willfarrell/docker-autoheal project. The difference is in additional parameter `AUTOHEAL_RESTART_TIMEOUT` to wait until an unhealthy container is restarted. This addition solves two problems:
* It is possible to stop a container without stopping autoheal.
* There is time for a container to become healthy itself in case of temporary problems with external resources without sequential restart. Especially important if a service needs some time to 'warm up'.  

Monitor and restart unhealthy docker containers. 
This functionality was propose to be included with the addition of `HEALTHCHECK`, however didn't make the cut.
This container is a stand-in till there is native support for `--exit-on-unhealthy` https://github.com/docker/docker/pull/22719.

## Supported tags and Dockerfile links
- [`latest` (*Dockerfile*)](https://github.com/ardas/docker-autoheal/blob/master/Dockerfile)

[![](https://images.microbadger.com/badges/version/willfarrell/autoheal.svg)](http://microbadger.com/images/willfarrell/autoheal "Get your own version badge on microbadger.com")  [![](https://images.microbadger.com/badges/image/willfarrell/autoheal.svg)](http://microbadger.com/images/willfarrell/autoheal "Get your own image badge on microbadger.com")

## How to use
```bash
docker run -d \
    --name autoheal \
    --restart=always \
    -e AUTOHEAL_CONTAINER_LABEL=all \
    -v /var/run/docker.sock:/var/run/docker.sock \
    ardas/autoheal
```
a) Apply the label `autoheal=true` to your container to have it watched.

b) Set ENV `AUTOHEAL_CONTAINER_LABEL=all` to watch all running containers. 

c) Set ENV `AUTOHEAL_CONTAINER_LABEL` to existing label name that has the value `true`.

Note: You must apply `HEALTHCHECK` to your docker images first. See https://docs.docker.com/engine/reference/builder/#/healthcheck for details.

## ENV Defaults
```
AUTOHEAL_CONTAINER_LABEL=autoheal
AUTOHEAL_RESTART_TIMEOUT=90   # restart if a container is unhealthy during 90 seconds
AUTOHEAL_INTERVAL=5   # check every 5 seconds
DOCKER_SOCK=/var/run/docker.sock
```

## Testing
```bash
docker build -t autoheal .

docker run -d \
    -e AUTOHEAL_CONTAINER_LABEL=all \
    -v /var/run/docker.sock:/var/run/docker.sock \
    autoheal                                                                        
```
