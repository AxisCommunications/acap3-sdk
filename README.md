# Quick reference

[ACAP Developer Guide – Version 3](https://help.axis.com/acap-3-developer-guide)

- Supported architectures: amd64

## Overview

This image is based on Ubuntu and contains the environment needed for building an AXIS Camera Application Platform (ACAP) application.
It includes all tools for building and packaging an ACAP 3 application as well as API components (header and library files) needed for accessing different parts of the camera firmware. The image can be used as a basis for custom built images to run your application or as a developer environment inside the container.

For more information on the latest version and what's new, see [What's new in ACAP SDK](https://help.axis.com/acap-3-developer-guide#whats-new-in-acap-sdk). For more information about Axis ́APIs, SDKs, technical tools and extensive technical documentation, see [Developer Community](https://www.axis.com/developer-community/acap).

See the license section at the bottom of this page for restrictions that relate to the use of this image.

## ACAP

[AXIS Camera Application Platform (ACAP)](https://www.axis.com/sv-se/products/analytics/acap), is an open platform supported by most Axis cameras. The platform enables developers to develop applications that can be downloaded and installed on Axis network cameras and video encoders.

# How to build the ACAP SDK image

## armv7hf

```sh
docker build --build-arg ARCH=armv7hf -t <your_tag> .
```

## aarch64

```sh
docker build --build-arg ARCH=aarch64 -t <your_tag> .
```

# How to use the ACAP SDK image

An example of how to use the ACAP SDK image to build a [hello-world application](https://github.com/AxisCommunications/acap3-examples/tree/master/hello-world) is found on Github.

For more information on building and installing an application, see [ACAP Developer Guide – Version 3](https://help.axis.com/acap-3-developer-guide#build-install-and-run-the-application).

# License

By downloading ACAP SDK you automatically agree to the terms in the [license agreement](https://www.axis.com/techsup/developer_doc/EULA/LICENSE.pdf)

ACAP SDK open source licenses and copyleft source code are found [here](http://acap-artifacts.s3-website.eu-north-1.amazonaws.com/)
