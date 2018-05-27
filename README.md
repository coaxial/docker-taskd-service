# taskd docker service

A [taskwarrior](https://taskwarrior.org) taskserver in a ~~box~~ container.

## Getting started

This docker service will generate the necessary certificates, configure, and start a `taskd` server. The variables are set with environment variables.

Setting up a task server is pretty tricky, I have tried to strike a balance between sane defaults and configurability.

### Settings

While the default values are perfectly fine, you might want to customize them. The file at `taskserver/taskserver.env.example` is all you need to edit. It is commented so I won't repeat the configuration variables here. Save your version as `taskserver/taskserver.env`.

> Note that the `CN` parameter is not configurable because it has to match exactly the `hostname -f` of the host running `taskd`. Since docker containers' hostnames are randomly generated, I had to pin the hostname to `taskserver`. See below how to override the hostname.

### Overriding the FQDN

Unless you are running taskwarrior from a container on the same network as the `taskserver` service defined in this repo, you won't be able to sucessfully sync with it. This is because `taskserver` isn't a fully qualified domain name and certainly won't resolve with external DNS servers.

As a workaround, the actual FQDN can be overridden using `docker-compose.hostname-override.yml.example`. The service can then be started using both docker-compose files (cf. https://docs.docker.com/compose/extends/)

## Starting the server

After editing the variables and overriding the FQDN, start the service:

`$ docker-compose -f docker-compose.yml -f docker-compose.hostname-override.yml up -d`

The generated user certificates will be located at `taskserver/client_certs` as `username.{cert,key}.pem` and the UUID in the `username-uuid` file. The directory also contains the `ca.cert.pem` file.

## About the container

The container is based on my `coaxial/taskserver` image which is based on Alpine. The base image is rebuilt whenever `alpine` is updated to ensure an up to date environment.

For process supervision, I'm using s6-overlay. The taskd service is defined at `taskserver/root/etc/services.d/taskd/run`. Several scripts in `taskserver/root/etc/cont-init.d` generate the certificates and configures the server before it starts. The logs are writted to the `taskddata` volume, in a `taskd.log` file. To also have the logs output to the container's stdout, I set up a `taskd-log` service at `taskserver/root/etc/services.d/taskd-log/run` which tails the log file to stdout.

The s6-overlay repo has a README to explain how it works in more details. You can find it here: https://github.com/just-containers/s6-overlay
