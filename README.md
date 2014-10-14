Docker Registry plugin for Dokku
--------------------------------

The Docker registry plugin for [Dokku](https://github.com/progrium/dokku) adds support for managing application releases as part of a continuous delivery workflow. 

The idea is to build a Docker image once and deploy it to various environments (e.g staging ->  production), on Heroku similar functionality is offered through the [pipeline feature] (https://devcenter.heroku.com/articles/labs-pipelines). 

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

```

Workflow
--------

TODO
