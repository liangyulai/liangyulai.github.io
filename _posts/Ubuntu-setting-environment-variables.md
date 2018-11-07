---
title: 'Ubuntu: setting environment variables'
date: 2016-08-01 14:59:12
tags:
  - Ubuntu
---

#### Setting Environment Variable

* Setting values to environment variables

    ```
    ~ ❯ PORT=3333
    ~ ❯ export PORT
    ~ ❯ export EDITOR=nano
    ```

* Examining values of environment variables

    ```
    ~ ❯ printenv PORT
    ```

* Erasing environment variables

    ```
    ~ ❯ export PORT=
    ~ ❯ unset PORT
    ```

* Reference: [Environment Variables](https://help.ubuntu.com/community/EnvironmentVariables)

#### Checking your Ubuntu Version

* Reference: [Checking your Ubuntu Version](https://help.ubuntu.com/community/CheckingYourUbuntuVersion)
    ```
    ~ ❯ lsb_release -a
    No LSB modules are available.
    Distributor ID:	Ubuntu
    Description:	Ubuntu 14.04.4 LTS
    Release:	14.04
    Codename:	trusty
    ```
