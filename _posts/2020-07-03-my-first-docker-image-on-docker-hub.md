---
title: My first Docker image on Docker Hub
date: 2020-07-03 08:46:00 +02
tags: ["FOP","XML","Docker","Java","Ubuntu"]
---

Based on my [previous post about building Apache FOP in a Docker image](/2020/06/08/running-apache-fop-in-docker.html), I now have [my first image on Docker Hub](https://hub.docker.com/r/chrwahl/fop).

It is more or less a copy of the Dockerfile from the post, so what is new?
First of all it is now easier to make use of the Docker image be course it is already build.
You just need to pull the image:

```sh
$ docker pull chrwahl/fop
```
And then you are ready for generating PDF files:

```sh
$ docker run --user $(id -u):$(id -g) -v $(pwd):/src -w /src -it --rm chrwahl/fop -xml data.xml -xsl fo.xslt -pdf document.pdf
```

Second, the build process also includes a [repository on Github](https://github.com/chrwahl/docker-fop).
For now I just have one Dockerfile, but I would like to extend the project with more, so that different versions of FOP can be installed.