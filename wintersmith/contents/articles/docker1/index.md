---
title: Having fun with Docker
author: Devric
date: 2014-04-16 15:00
template: article.jade
---

![Docker](docker_logo.png)

Recently clean installed my mac, but there is still some issues with my graphic card, probably the logic board :( 

... well too expensive to fix it if it is.

<span class="more"></span>

Anyway, since it's a clean install and I'm kind of tired of managing all the environments like nginx, php, node, ruby, python, and different databases directly in my mac, and not that happy with using so many different boxes of vagrants, it just eating up my machines resources! 

So I finally start to look into LXC containers with docker, and yup it is just F....... awesome. A whole linux development or production environents can be boot it up within minutes, and very portable.

Since I'm on a mac, a VM box is required to run docker with, I started to go with a ubuntu box and installed docker with it, but it's quite slow for the booting, so I went for boot2docker.

Fiddling around with boot2docker for a day, this VM is very lightweight, and was design to run purely on ram which is blazing fast to use, however the current version can't have share drives through the VM, only hackrish fix is to mount a folder within the VM to my host, instead of tring to make the NFS working with boot2docker, I gave up, and switched for coreos.

It's not as fast as boot2docker, but it is still a lightweight VM, and have NFS ready out of the box! FK..... love it! it is build for docker! I've been using it for a while now. Should start to write some articles about it soon.
