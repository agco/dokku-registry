
registry:login                                  Perform a docker login ( all the remaining args are propagated )
registry:push <git_url> <docker_registry_image> Clone a git url, exec buildstep and push the Docker image to the current registry
registry:pull <app> <docker_registry_image>     Pull a buildstep built Docker image from the current registry and deploy