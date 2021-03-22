# picamera-streaming-demo

As an update to the issue this fork was made to fix, fixing it is actually fairly easy on the snapcraft/this repo's side of things. 
The modification needed lies in the `snapcraft.yaml` file as follows:

```
architectures:
  - build-on: armhf
    run-on: armhf
```
changes to 
```
architectures:
  - build-on: arm64
    run-on: arm64
```
or
```
architectures:
  - build-on: arm64
    run-on: all
```
These changes have already been pushed to this repo and in theory this should work great, but in practice it doesn't work due to an issue upstream. This project relies on the python package 
`picamera` [(readthedocs)](https://picamera.readthedocs.io/) which communicates with the raspberry pi camera hardware through mmal (multimedia abstraction layer) 
which is provided by the raspberry pi userland [(userland on github)](https://github.com/raspberrypi/userland). From what I've tested out, you can install userland 
on a 64-bit version of Ubuntu 20.XX desktop on the raspberry pi 4, and it will let you use Raspistill without issue as if you were running on raspberry pi OS/raspbian. 
You can even install `picamera` in a 64-bit environment with a little effort. 

Unfortunately, MMAL is apparently only compatible with 32-bit userland and will thus cause `picamera` to fail when used. 

## Going forward 

Does this mean 64-bit support for this is a lost cause? Not at all, the creator of `picamera` says that MMAL is now considered legacy and thus `picamera` 
itself can be considered legacy (although they did state that they are still working on the package). It seems work has shifted from MMAL to 
libcamera [(homepage)](https://www.libcamera.org/) and I'm looking into replacing `picamera` in this project with `picamerax` or checking if 
libcamera is ready to replace `picamera for this.

When I do find a path to achieving 64-bit support I will update things here with a how-to of going from a clean install of an OS to running this project.
