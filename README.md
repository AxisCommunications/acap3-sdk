# Quick reference
[ACAP Developer Guide – Version 3](https://www.axis.com/products/online-manual/s00004)

- Supported architectures: amd64


## Overview
This image is based on Ubuntu and contains the environment needed for building an AXIS Camera Application Platform (ACAP) application.
It includes all tools for building and packaging an ACAP 3 application as well as API components (header and library files) needed for accessing different parts of the camera firmware. The image can be used as a basis for custom built images to run your application or as a developer environment inside the container.

For more information on the latest version and what's new, see [What's new in ACAP SDK](https://www.axis.com/products/online-manual/s00004#t10160830). For more information about Axis ́APIs, SDKs, technical tools and extensive technical documentation, see [Developer Community](https://www.axis.com/developer-community/acap).

See the license section at the bottom of this page for restrictions that relate to the use of this image.


## ACAP
[AXIS Camera Application Platform (ACAP)](https://www.axis.com/sv-se/products/analytics/acap), is an open platform supported by most Axis cameras. The platform enables developers to develop applications that can be downloaded and installed on Axis network cameras and video encoders.


# How to use the ACAP SDK image
An example of how to use the ACAP SDK image to build a [hello-world application](https://github.com/AxisCommunications/acap3-examples/tree/master/hello-world) is found on Github.


## Build application inside container

Standing in your working directory run the following command to bind mount the application directory in to the acap-sdk container:

     docker run --rm -v $PWD/app:/opt/app -it hello_world:1.0 axisecp/acap-sdk:3.3-armv7hf-ubuntu20.04

Now inside the container to build the application run

     create-package.sh

The app directory now contains the following files:

    ├── Dockerfile
    └── app
        ├── LICENSE
        ├── Makefile
        ├── hello_world
        ├── hello_world.c
        ├── hello_world.o
        ├── hello_world_1_0_0_LICENSE.txt
        ├── hello_world_1_0_0_armv7hf.eap
        ├── package.conf
        ├── package.conf.orig
        └── param.conf

To install the application to a camera use command

      eap-install.sh --help

For more information on building and installing an application, see [ACAP Developer Guide – Version 3](https://www.axis.com/products/online-manual/s00004#t10152940).


# License

By downloading ACAP SDK you automatically agree to the terms in the [license agreement](https://www.axis.com/techsup/developer_doc/EULA/LICENSE.pdf)

ACAP SDK open source licenses and copyleft source code are found [here](http://acap-artifacts.s3-website.eu-north-1.amazonaws.com/)
