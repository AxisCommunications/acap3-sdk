# Quick reference
[ACAP3 Manual](https://www.axis.com/products/online-manual/s00004)

- Supported architectures: amd64


## Overview
This image is based on Ubuntu and contains the environment needed for building an AXIS Camera Application Platform (ACAP) application.
It includes all tools for building and packaging an ACAP 3 application as well as API components (header and library files) needed for accessing different parts of the camera firmware. The image can be used as a basis for custom built images to run your application or as a developer environment inside the container.

For more information on the latest version and what's new, see [What's new in ACAP SDK](https://www.axis.com/products/online-manual/s00004#t10160830). For more information about Axis ́APIs, SDKs, technical tools and extensive technical documentation, see [Developer Community](https://www.axis.com/developer-community/acap).

See the license section at the bottom of this page for restrictions that relate to the use of this image.


## ACAP
[AXIS Camera Application Platform (ACAP)](https://www.axis.com/sv-se/products/analytics/acap), is an open platform supported by most Axis cameras. The platform enables developers to develop applications that can be downloaded and installed on Axis network cameras and video encoders.


# How to use the ACAP SDK Image
An example of how to use the ACAP SDK image to build a hello-world application


## Create a Hello World application
Create the following folder and file structure in a working directory:

    ├── Dockerfile
    └── app
        ├── LICENSE
        ├── Makefile
        └── hello_world.c


The files comprising the following:

__Dockerfile__

    FROM axisecp/acap-sdk:3.2-armv7hf-ubuntu20.04

    # Building the ACAP application
    COPY ./app /opt/app/
    WORKDIR /opt/app
    RUN create-package.sh

__Makefile__ _(Note! make sure to preserve the tabs below, recipes in a makefile must be preceded by a single standard tab character)_

    PROGS = hello_world
    SRCS  = hello_world.c
    OBJS  = $(SRCS:.c=.o)
    all: $(PROGS)
    $(PROGS): $(OBJS)
      $(CC) $^ -o $@
    clean:
      rm -f $(PROGS) *.o

__LICENSE__

    Third Party Software Licenses


__hello_world.c__

    #include <syslog.h>

    void main(int argc, char *argv[]) {
        /* Open the syslog to report messages for "hello_world" */
        openlog("hello_world", LOG_PID|LOG_CONS, LOG_USER);
        /* Choose between { LOG_INFO, LOG_CRIT, LOG_WARN, LOG_ERR }*/
        syslog(LOG_INFO, "Hello, World!");
        /* Close application logging to syslog */
        closelog();
    }


## Build and run the application

Standing in your working directory run the following commands:

     docker build --tag hello_world:1.0 .

Copy the result from the build to a local directory `build`

    docker cp $(docker create hello_world:1.0):/opt/app ./build

The working directory now contains a build folder with the following files:

    ├── Dockerfile
    ├── app
    │   ├── LICENSE
    │   ├── Makefile
    │   └── hello_world.c
    └── build
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


## Install your application on an Axis video product

Installing your application on an Axis video product is as simple as:

- Browse to the following page (replace `<axis_device_ip>` with the IP number of your Axis video product)

    `http://<axis_device_ip>/#settings/apps`

- Click Add, the `+` sign and browse to the newly built `hello_world_1_0_0_armv7hf.eap`

- Click `Install`

- `hello_world` is now available as an application on the device.

- Run the application by clicking on the application icon and enable the `Start` switch


## Build application inside container

Standing in your working directory run the following command to bind mount the application directory in to the acap-sdk container:

     docker run --rm -v $PWD/app:/opt/app -it hello_world:1.0 axisecp/acap-sdk:3.2-armv7hf-ubuntu20.04

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

For more information on building and installing an application, see [ACAP3 Manual](https://www.axis.com/products/online-manual/s00004#t10152940).


# License

By downloading AXIS Embedded Development SDK you automatically agree to the terms in the [license agreement](https://www.axis.com/techsup/developer_doc/EULA/LICENSE.pdf)

