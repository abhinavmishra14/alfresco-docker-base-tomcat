# Alfresco Docker Base Tomcat [![Build Status](https://img.shields.io/github/actions/workflow/status/Alfresco/alfresco-docker-base-tomcat/main.yml?branch=master)](https://github.com/Alfresco/alfresco-docker-base-tomcat/actions/workflows/main.yml)

This repository contains the [Dockerfile](Dockerfile) used to create the parent
Tomcat image that will be used by Alfresco engineering teams, other internal
groups in the organization, customers and partners to create Tomcat bases
application images from.

Images are available for latest Tomcat 8.5.x and 9.0.x.

This image depends on the [alfresco-docker-base-java](https://github.com/Alfresco/alfresco-docker-base-java).

## Quickstart

Choose between one of the available flavours built from this repository:

Tomcat version | Java version | Java flavour | OS               | Image tag                    | Size
---------------|--------------|--------------|------------------|------------------------------|-------------------------------------
9              | 11           | jre          | Centos 7         | tomcat9-jre11-centos7        | ![tomcat9-jre11-centos7 size][1]
9              | 11           | jdk          | Centos 7         | tomcat9-jdk11-centos7        | ![tomcat9-jdk11-centos7 size][2]
8              | 11           | jre          | Centos 7         | tomcat8-jre11-centos7        | ![tomcat8-jre11-centos7 size][3]
8              | 11           | jdk          | Centos 7         | tomcat8-jdk11-centos7        | ![tomcat8-jdk11-centos7 size][4]
9              | 11           | jre          | Rocky Linux 8    | tomcat9-jre11-rockylinux8    | ![tomcat9-jre11-rockylinux8 size][5]
8              | 11           | jre          | Rocky Linux 8    | tomcat8-jre11-rockylinux8    | ![tomcat8-jre11-rockylinux8 size][6]
9              | 17           | jre          | Rocky Linux 8    | tomcat9-jre17-rockylinux8    | ![tomcat9-jre17-rockylinux8 size][7]
8              | 17           | jre          | Rocky Linux 8    | tomcat8-jre17-rockylinux8    | ![tomcat8-jre17-rockylinux8 size][8]

[1]: https://img.shields.io/docker/image-size/alfresco/alfresco-base-tomcat/tomcat9-jre11-centos7
[2]: https://img.shields.io/docker/image-size/alfresco/alfresco-base-tomcat/tomcat9-jdk11-centos7
[3]: https://img.shields.io/docker/image-size/alfresco/alfresco-base-tomcat/tomcat8-jre11-centos7
[4]: https://img.shields.io/docker/image-size/alfresco/alfresco-base-tomcat/tomcat8-jdk11-centos7
[5]: https://img.shields.io/docker/image-size/alfresco/alfresco-base-tomcat/tomcat9-jre11-rockylinux8
[6]: https://img.shields.io/docker/image-size/alfresco/alfresco-base-tomcat/tomcat8-jre11-rockylinux8
[7]: https://img.shields.io/docker/image-size/alfresco/alfresco-base-tomcat/tomcat9-jre17-rockylinux8
[8]: https://img.shields.io/docker/image-size/alfresco/alfresco-base-tomcat/tomcat8-jre17-rockylinux8

* [Docker Hub](https://hub.docker.com/r/alfresco/alfresco-base-tomcat) image name: `alfresco/alfresco-base-tomcat`
* [Quay](https://quay.io/repository/alfresco/alfresco-base-tomcat) image name: `quay.io/alfresco/alfresco-base-tomcat`

Example final image: `alfresco/alfresco-base-tomcat:tomcat9-jre11-centos7`

> If you are using this base image in a public repository, please stick to the Docker Hub published image.

### Image pinning

The [pinning suggestions provided in alfresco-base-java](https://github.com/Alfresco/alfresco-docker-base-java/blob/master/README.md#image-pinning) are valid for this image too.

## Usage

The image can be used via `docker run` to run java applications with `--read-only` set.

Depending on your use case, you may want to set the following path as volumes:

* `/usr/local/tomcat/logs`
* `/usr/local/tomcat/work`
* `/usr/local/tomcat/temp`
* `/usr/local/tomcat/conf/Catalina`

The Tomcat in this image is running with Security Manager switched on. This may
impact performance. The Security Manager can be disabled by overriding the
startup command to:

```bash
CMD ["catalina.sh", "run"]
```

## Development

### Naming specs

The images built from this repository are named as follow:

`tomcat<TOMCAT_VERSION>-<JAVA_DISTRIBUTION_TYPE><JAVA_MAJOR_VERSION>-<OS_DISTRIBUTION_NAME><OS_DISTRIBUTION_VERSION>`

### How to build an image locally

To build this image, run the following script:

```bash
IMAGE_REPOSITORY=alfresco/alfresco-base-tomcat
docker buildx build -t $IMAGE_REPOSITORY . \
  --build-arg DISTRIB_NAME=$DISTRIB_NAME \
  --build-arg DISTRIB_MAJOR=$DISTRIB_MAJOR \
  --build-arg JAVA_MAJOR=$JAVA_MAJOR \
  --build-arg JDIST=$JDIST \
  --build-arg TOMCAT_MAJOR=$TOMCAT_MAJOR \
  --no-cache
```

where:

* DISTRIB_NAME is centos or rockylinux
* DISTRIB_MAJOR is 7 for centos or 8 for rockylinux
* JAVA_MAJOR is 11 or 17 for rockylinux only
* JDIST is jre or jdk
* TOMCAT_MAJOR is 8 or 9

### Release

Just push a commit on the default branch including `[release]` in the message to trigger a release on the CI.
