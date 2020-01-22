---
title: How to save Docker logs in MongoDB with Fluentd
date: 2020-01-22T19:12:41.988Z
tags:
  - raspberrypi
  - docker
  - fluentd
---
Sometimes it is needed to aggregate or simply save the logs of container applications somewhere outside the docker host machine. For example, when using a small device like a raspberry pi with limited storage.

In this guide we will setup fluentd (https://www.fluentd.org) in order to stream container logs to a remote mongoDB database.

#### Fluentd installation

*Note: In case you are running a 64 bits Linux machine, there are already fluentd docker images and binaries, so you can take a look and skip this part.*

In order to install fluentd on a raspberrypi, first we need to make sure we have some dependencies:
`sudo apt-get update
sudo apt-get install build-essential autoconf libc6-dev automake libtool ruby-dev`

Then we need to install fluentd as a ruby gem:
`sudo gem install fluentd`

And the fluentd plugins that we will use:
`sudo fluent-gem install fluent-plugin-td`
`sudo fluent-gem install fluent-plugin-mongo`

That's all, now fluentd is installed

##### Fluentd configuration
