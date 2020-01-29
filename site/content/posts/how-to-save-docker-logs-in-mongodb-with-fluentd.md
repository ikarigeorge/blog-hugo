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

```
sudo apt-get update
sudo apt-get install build-essential autoconf libc6-dev automake libtool ruby-dev
```

Then we need to install fluentd as a ruby gem:

```
sudo gem install fluentd
```

And the fluentd plugins that we will use:

```
sudo fluent-gem install fluent-plugin-td
```

```
sudo fluent-gem install fluent-plugin-mongo
```

That's all, now fluentd is installed

##### Fluentd configuration

Fluentd needs a configuration file where we tell what are the inputs and outputs we want. Since docker can communicate with our fluentd process, the config is pretty simple. Create a fluent.conf file and configure the mongoDB info as shown in the fluent official documentation ( https://docs.fluentd.org/output/mongo ):

```
<source>
  @type forward
</source>
<match docker.*>
  # plugin type
  @type mongo
  # connection string can also be used: 
  # connection_string mongodb://<User>:<Pass>@localhost:27017/myDatabase?w=0
  # mongodb db + collection
  database myDatabase
  collection logs
  # mongodb host + port
  host localhost
  port 27017
  # authentication
  user michael
  password jordan
  # interval
  <buffer>
    flush_interval 10s
  </buffer>
  # make sure to include the time key
  <inject>
    time_key time
  </inject>
</match>
```

The important thing here is the match expression, we are saying that our logs will have a tag of the style "docker." + other thing, for example the container name.

Now we can start fluentd with the command:

```
fluentd -c fluent.conf
```

#### Docker log output

When running docker containers, we need to tell them that we will use fluentd instead of the standard log mechanism. And we need to specify the correct tag so fluentd saves the logs. For example, lets run a simple alpine container:

```
sudo docker run --log-driver=fluentd --log-opt tag="docker.{{.ID}}" alpine echo﻿ 'Hello world!'
```

The tag option can include the container id like in the example, or other parameters, as documented in the official docker page ( https://docs.docker.com/config/containers/logging/log_tags/ ). 

If everything went fine, you should see an entry in your mongoDB collection. Try other containers you own and you can see how they log in the database :)

#### Extra: Setting fluentd as service

If you want that fluentd starts automatically after starting the computer, you need to add the `fluentd.service` file into `/etc/systemd/system`:

```
Description=Fluentd
Documentation=http://www.fluentd.org/
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/fluentd -d /run/fluentd.pid -c /path/to/fluent.conf
PIDFile=/run/fluentd.pid
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
