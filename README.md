# elkpi-yocto-layers

Repo Manifest file for building Elk Audio OS Images for the Elk Pi Development Kit using the Yocto/Openembedded build system.

## Prerequisites

In order to use the manifest file, you need to install Google's `repo` tool. Installation instructions can be found [here](https://gerrit.googlesource.com/git-repo).

You will also need a host Linux machine with a reasonably recent distribution and at least 60 GB of disk space available. Check the [official Yocto requirements](https://docs.yoctoproject.org/ref-manual/system-requirements.html) for more details.
[Docker](https://www.docker.com/) can be used for building under Windows / macOS without installing a Linux Virtual Machine, although there will still be a non-negligible impact on build times compared to native Linux builds. Another popular option is to host the build environment on the cloud using Amazon's EC2 instances, Microsoft Azure or similar services.

Other than mentioned on the official Yocto requirements, the build does not work on Ubuntu 22.04 LTS.
On Ubuntu 20.04 LTS, you need to downgrade to Python 3.9.X for a successful build.

Required git configuration
```
git config --global url.https://github.com/.insteadOf git://github.com/
```

Follow these [hints](https://docs.yoctoproject.org/dev/dev-manual/speeding-up-build.html) for speeding up the build.

You will also need to install [gRPC 1.10.0](https://github.com/grpc/grpc/tree/v1.10.x) and [protobuf 3.5.0](https://github.com/protocolbuffers/protobuf/tree/3.5.x) on your host environment.

## Usage

Initialize the layers using `repo init`. For example, this will use the manifest at the head of this repository:

```bash
repo init -u https://github.com/elk-audio/elkpi-yocto-layers.git
```

To check out a particular version tag:

```bash
repo init -u https://github.com/elk-audio/elkpi-yocto-layers.git -b regs/tag/<tag name>
```

After the initialization step, run the following command to fetch all the required layers:

```bash
repo sync
```

## Updating the layers

When a new version is released and the manifest file is updated, you can simply re-run `repo init` and `repo sync` as described in the previous steps. This will synchronize all the layers to the right commit without needing to refetch everything.

## Setting Up Yocto

After the layers are initialized with `repo`, you should see a new directory named `layers` in your current path.

Run the following command from the same directory as you ran `repo`:


```bash
TEMPLATECONF=./meta-elkpi/conf source layers/oe-init-build-env
```

This will set up the enviroment and you will end up in a directory called `build`. Now you can build the image as described in [meta-elkpi](https://github.com/elk-audio/meta-elkpi):

```bash
bitbake elkpi-audio-os-image
```

Be careful that the process to build an image from scratch can take up to several hours depending on your machine configuration. Following builds are usually much faster, since most of the heavy-weight components change rarely between updates of Elk Audio OS.

---
Copyright 2017-2019 Modern Ancient Instruments Networked AB, dba Elk, Stockholm, Sweden.
