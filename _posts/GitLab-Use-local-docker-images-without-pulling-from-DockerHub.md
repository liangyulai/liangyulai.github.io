---
title: 'GitLab: Use local docker images without pulling from DockerHub'
date: 2016-08-01 14:53:33
tags:
  - GitLab
  - Docker
---


#### Use local images without pulling from Docker

* If you are using local docker image, which is not available on Docker registry
* It will try to pull from the Docker registry and then fall back to the local
* It is kind of waste time

```
gitlab-ci-multi-runner 1.0.4 (014aa8c)
Using Docker executor with image ubuntu:v4 ...
Pulling docker image ubuntu:v4 ...
WARNING: Cannot pull the latest version of image ubuntu:v4 : Tag v4 not found in repository docker.io/library/ubuntu
WARNING: Locally found image will be used instead.

Running on runner-6fff690c-project-62-concurrent-0 via Himalayas...
```

* Disable the Docker image pull

```
~ ❯ sudo cat /etc/gitlab-runner/config.toml
concurrent = 1

[[runners]]
  name = "your-docker-name"
  url = "your-gitlab-ci-address/ci"
  token = "your-runner-token"
  executor = "docker"
  [runners.docker]
    image = "ruby:2.1"
    privileged = false
    volumes = ["/cache"]
    image_ttl = 0
```

* It will save several seconds here (from 11s down to 6s):

```
gitlab-ci-multi-runner 1.0.4 (014aa8c)
Using Docker executor with image ubuntu:v4 ...

Running on runner-6fff690c-project-62-concurrent-0 via Himalayas...
```

* Reference: [GitLab Runner Changelog](https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/blob/master/CHANGELOG.md)
  * v 1.1.0
    * Add docker-pull-policy, it removes the docker-image-ttl
  * v 1.0.0
    * Allow to specify ImageTTL for configuration the frequency of docker image re-pulling (see advanced-configuration)
