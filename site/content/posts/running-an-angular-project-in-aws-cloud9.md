---
title: Running an angular project in AWS Cloud9
date: 2020-01-10T13:06:20.498Z
tags:
  - angular
  - javascript
  - aws
  - cloud9
---
Since Cloud9 was acquired by AWS, few changes made the development of Angular or any web application a little bit trickier. The following steps are necessary to visualize or share a running angular development application:

First, we need to change the EC2's security parameters, as described in this post: https://community.c9.io/t/what-is-vfs-connection-does-not-exist-on-aws-c9/22697/17

1. First, click on your user icon and then in Manage EC2 instance
2. In the Description tab, click in the security group link.
3. Add a new inbound rule
4. Set the rule with the following parameters: 

   * Type: Custom TCP
   * Port Range: The port of the app (by default 4200)
   * Source: Anywhere (Warning: this makes your app public to the world, for local dev you can choose "My IP", but beware normally your internet provider will change this IP regularly and you need to repeat this step again)
5. Click Save

Next, instead of just running `ng serve` to start the angular app, run or add to package.json the following command:ng

```
ng serve --host 0.0.0.0 --disableHostCheck
```

This will make the app available outside the EC2 instance and ignore that we will access it through the EC2's public IP.

Now you should be able to open a new tab and view your app!

![](/img/2020-01-10_1.png)
