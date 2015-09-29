# dokku-registry [![Build Status](https://img.shields.io/travis/agco/dokku-registry.svg?branch=master "Build Status")](https://travis-ci.org/agco/dokku-registry)

The Docker registry plugin for [Dokku](https://github.com/progrium/dokku) adds support for managing application releases as part of a continuous delivery workflow.

On Heroku similar functionality is offered by the [heroku-labs pipeline feature](https://devcenter.heroku.com/articles/labs-pipelines), which allows you to promote builds across multiple environments (staging -> production)

## requirements

- dokku 0.4.0+
- docker 1.8.x

## installation

```shell
# on 0.3.x
cd /var/lib/dokku/plugins
git clone https://github.com/agco/dokku-registry.git registry
dokku plugins-install

# on 0.4.x
dokku plugin:install https://github.com/agco/dokku-registry.git registry
```

## Commands

```
$ dokku help
    registry:login <docker login args>              Login to a docker registry
    registry:push <git_url> <docker_registry_image> Clone a git url, exec buildstep and push the resulting Docker image to the logged on registry
    registry:pull <app> <docker_registry_image>     Pull a Docker image (produced with buildstep) from the logged on registry and deploy
```

## Workflow


### Build machine

```
dokku registry:login —username=<username> —password=<password> —email=<email> <server>
```
This logs you in to the dockerhub registry
Other registries can be specified as well with using the optional server arg
Check the [docker login](https://docs.docker.com/reference/commandline/cli/#login) docs for more details

```
dokku registry:push <git_url> <image_tag>
```
This clones the code from a given git_url and builds it with buildstep
The resulting Docker is tagged as specified by the image_tag arg and pushed to the current logged-on Docker registry

### Deploy target(s)

```
dokku registry:login —username=<username> —password=<password> —email=<email> <server>
```

```
dokku registry:pull <appname> <image_tag>
```
This pulls the image tag from the current logged-on registry and deploys it to Dokku. The application will be auto-created in case it doesn't exist yet.

### Quick example

Note : This is just to demonstrate syntax, replace credentials and registry image tag with your own values

```
dokku registry:login --username=myusername --email=myemail@mailinator.com --password=mypwd
dokku registry:push https://github.com/heroku/node-js-getting-started.git myusername/myapp:1.2.3
dokku registry:pull myapp myusername/myapp:1.2.3
```

### Set config variables prior to application start

The workflow then looks something like the following

```
dokku registry:login --username=myusername --email=myemail@mailinator.com --password=mypwd
dokku create myapp
dokku config:set myapp FOO=BAR
dokku registry:pull myapp myusername/myapp:1.2.3
```

An example on how to bootstrap this can be found here : https://github.com/agco-adm/dokku-provision-ALM-support


