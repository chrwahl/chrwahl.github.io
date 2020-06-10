---
title: Running Apache FOP in Docker
date: 2020-06-08 17:21:00 +02
tags: ["FOP","XML","Docker","Java","Ubuntu"]
---

One of my interests is XML and with that interest, a tail of related technologies. Over the years I have spend a lot of time on XSL transforming "stuff".
From time to time I have also used [Apache FOP](https://xmlgraphics.apache.org/fop/){:target="blank"}, a tool for generating PDF files from XML using XSL formatting objects (XSL-FO).
This tool require Java – like everything else – back then. But now, not so much. So, actually I don't have Java installed.

The question is: how can I run FOP without all the hassle of installing Java? Here Docker comes to the rescue.
From previous experiences with FOP I knew that I needed [Sun/Oracle Java](https://www.java.com).
I don't know the details, but fortunately FOP version 2.5 can also run under [OpenJDK](https://openjdk.java.net/). This made it even easier to get up and running.

Before getting into the details – this is the final Dockerfile:

```sh
FROM ubuntu

ENV PATH "$PATH:/usr/local/fop-2.5/fop"

RUN apt-get update
RUN apt-get install -y default-jre
RUN apt-get install -y wget

RUN wget http://ftp.download-by.net/apache/xmlgraphics/fop/binaries/fop-2.5-bin.tar.gz

RUN tar -xvzf fop-2.5-bin.tar.gz -C /usr/local

ENTRYPOINT ["fop"]
CMD ["-version"]
```
Details:

1. The starting point is the latest [Ubuntu image](https://hub.docker.com/_/ubuntu): `FROM ubuntu`. I haven't looked into other options.
2. To be able to call fop from the shell I need to add the path of the installation to the PATH environment variable: `ENV PATH "$PATH:/usr/local/fop-2.5/fop"`. I struggled a lot with this, adding the path to /etc/profile and ~/.bashrc, and running both `source /etc/profile` and `. /etc/profile` etc. and finally ended with this simple solution.
3. I need both Java and wget installed. `RUN apt-get update` will download all the PPAs. And the running `RUN apt-get install -y default-jre` and `RUN apt-get install -y wget`, both with the -y flag will install the two applications. `default-jre` is an alias for OpenJDK.
4. Now, I can download the files for fop: `RUN wget http://ftp.download-by.net/apache/xmlgraphics/fop/binaries/fop-2.5-bin.tar.gz`.
5. And extract the tar.gz file into the `/usr/local` directory: `RUN tar -xvzf fop-2.5-bin.tar.gz -C /usr/local`.
6. Finally I add fop as the entrypoint: `ENTRYPOINT ["fop"]` and a default parameter: `CMD ["-version"]` to fop.

With the Docker file in place an image can be build:

```sh
$ docker build -t fop .
```

The container based on the image will just run as long as the process is running:

```sh
$ docker run -it --rm fop
```

#### Using Apache FOP
Just to give a practical (and short) example. This is the contents of helloworld.fo:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<root xmlns="http://www.w3.org/1999/XSL/Format">
  <layout-master-set>
    <simple-page-master margin="10mm" page-width="210mm" page-height="297mm" master-name="simple">
      <region-body region-name="simple-body" margin-bottom="20mm" margin-top="20mm" />
    </simple-page-master>
  </layout-master-set>
  <page-sequence master-reference="simple">
    <flow flow-name="simple-body">
      <block>Hello World!</block>
    </flow>
  </page-sequence>
</root>
```

To generate a PDF file execute:

```sh
$ docker run -v $(pwd):/src -w /src -it --rm fop helloworld.fo helloworld.pdf
```
Where `-v` is mapping the current directory to `/src`, `-w` is setting the working directory to `/src` and `fop  helloworld.fo helloworld.pdf` will generate a PDF file and save it in the working directory.

There are issues with the ownership of the generated PDF files, but I will look into that at a later point.