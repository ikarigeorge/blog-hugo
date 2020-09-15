---
title: Running an Electron app on a Raspberry Pi 4
date: 2020-09-11T23:21:52.805Z
tags:
  - rpi4
  - electron
  - javascript
---
When trying to run an Electron app in a raspberry pi, a blank window may appear if the gpu is not disabled.
This can be fixed at runtime with the command:
```
electron . --disable-gpu
```

However, when packaging or distributing your app, it may be necessary to disable the gpu from the code, to do that, you need to do the following before the app is ready:
```
app.disableHardwareAcceleration();
```
