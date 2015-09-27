Docker Registry plugin for Dokku
--------------------------------

The Docker registry plugin for [Dokku](https://github.com/progrium/dokku) adds support for managing application releases as part of a continuous delivery workflow.

On Heroku similar functionality is offered by the [heroku-labs pipeline feature](https://devcenter.heroku.com/articles/labs-pipelines), which allows you to promote builds across multiple environments (staging -> production)

Installation
------------
```
cd /var/lib/dokku/plugins
git clone https://github.com/agco-adm/dokku-registry
dokku plugins-install
```

Commands
--------
```
$ dokku help
    registry:login <docker login args>              Login to a docker registry
    registry:push <git_url> <docker_registry_image> Clone a git url, exec buildstep and push the resulting Docker image to the logged on registry
    registry:pull <app> <docker_registry_image>     Pull a Docker image (produced with buildstep) from the logged on registry and deploy
    registry:forcepull <app> <docker_registry_image>     Force pull a Docker image (produced with buildstep) from the logged on registry and deploy
```

Workflow
--------

## Build machine

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

## Deploy target(s)

```
dokku registry:login —username=<username> —password=<password> —email=<email> <server>
```

```
dokku registry:pull <appname> <image_tag>
```
This pulls the image tag from the current logged-on registry and deploys it to Dokku
The application will be auto-created in case it doesn't exist yet

## Quick example

Note : This is just to demonstrate syntax, replace credentials and registry image tag with your own values  

```
dokku registry:login --username=myusername --email=myemail@mailinator.com --password=mypwd
dokku registry:push https://github.com/heroku/node-js-getting-started.git myusername/myapp:1.2.3
dokku registry:pull myapp myusername/myapp:1.2.3
```

## Set config variables prior to application start

There is an outstanding PR which can accomodate this : https://github.com/progrium/dokku/pull/599/files

The workflow then looks something like the following

```
dokku registry:login --username=myusername --email=myemail@mailinator.com --password=mypwd
dokku create myapp
dokku config:set myapp FOO=BAR
dokku registry:pull myapp myusername/myapp:1.2.3
```

Awaiting the PR to get merged, here is a Dokku fork which has created a branch merging a couple of PRs with the 599 (create command) included https://github.com/neam/dokku/tree/awaiting-prs

An example on how to bootstrap this can be found here : https://github.com/agco-adm/dokku-provision-ALM-support
