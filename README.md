# ElkPi Yocto

Repo Manifest file for building Elk Audio OS Images for the Elk Pi Development Kit using the Yocto/Openembedded build system.

Downloads all the required layers necessary to set up the Yocto environment.

## Prerequisites
In order to use the manifest file, you need to install `repo`. Instructions to install can be found [here](https://gerrit.googlesource.com/git-repo).

## Usage

Initialize the `repo` with the followng command. This will use the manifest at the head of this repository:

```bash
repo init -u https://github.com/elk-audio/elkpi-yocto-layers.git
```

To check out a particular version tag :
```bash
repo init -u https://github.com/elk-audio/elkpi-yocto-layers.git -b regs/tag/<tag name>
```

Once it has been initialized, run the following command to fetch all the required layers.
```bash
repo sync
```

## Updating the layers
When a new version is released and the manifest file is updated, you can run the above two commands. This will synchorinize all the layers to point to the right commit without needing to refetch everything.

## Building the Image

If it your first time using Yocto to build the Elk Audio OS image for the ElkPi Dev Kit, you need to install the prerquisites as described [here](https://www.yoctoproject.org/docs/2.0/yocto-project-qs/yocto-project-qs.html).

You will also need to install [gRPC 1.10.0](https://github.com/grpc/grpc/tree/v1.10.x) and the [protobuf 3.5.0](https://github.com/protocolbuffers/protobuf/tree/3.5.x) in you build machine.

## Setting Up Yocto
If you have not already, use the `repo` command to fetch all the layers as described above. Once done, you should see a directory called `layers` from where you ran the repo command.

Run the following command from the same directory as you ran `repo`.

```bash
TEMPLATECONF=./meta-elkpi/conf source layers/oe-init-build-env
```

This will set up the enviroment and you will end up in a directory called `build`. Now you can build the image as described in [meta-elkpi](https://github.com/elk-audio/meta-elkpi).

```bash
bitbake elkpi-audio-os-image
```

Do note that first time builds may take a very long time (upto a few hours)