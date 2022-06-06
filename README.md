# Docker Image for Ubuntu with X11 and VNC

This is a Docker image for Ubuntu with X11 and VNC. The Ubuntu base containst the NVIDIA Drivers, CUDA, and Vulkan.

 - VNC is protected by a unique random password for each session
 - Desktop runs in a standard user account instead of the root account
 - Supports dynamic resizing of the desktop and 24-bit true color
 - Automatically shares the current work directory from host to Docker image
 - Is compatible with Singularity (tested with Singularity v2.6 and v3.2)

[![Build Status](https://travis-ci.org/x11vnc/docker-desktop.svg?branch=master)](https://travis-ci.org/x11vnc/docker-desktop)
[![Docker Image](https://images.microbadger.com/badges/image/x11vnc/desktop:master.svg)](https://microbadger.com/images/x11vnc/desktop)

![screenshot](https://raw.github.com/x11vnc/x11vnc-desktop/master/screenshots/screenshot.png)

## Preparation
Before you start, you need to first install Python, Docker, and nvidia-docker2.

## Building the Docker Image
```
sudo docker build -t x11vnc-desktop .
```

## Running the Docker Image
Enter the following command on the directory where the script x11vnc_desktop.py is (top level of the repository), and follow the instructions to connect through VNC.
```
python2 x11vnc_desktop.py -A "--runtime=nvidia -ti --rm -e DISPLAY" -i x11vnc-desktop
```

## Use with Singularity

This Docker image is constructed to be compatible with Singularity. This 
has been tested with Singularity v2.6 and v3.2. If you system does not yet have
Singularity, you may need to install it by following [these instructions](https://www.sylabs.io/guides/2.6/user-guide/quick_start.html#quick-installation-steps).
You must have root access in order to install Singularity, but you can use
Singularity as a regular user after it has been installed. If you do not
have root access, uou may need to ask your system administrator to install it for you.
It is recommended you use Singularity v2.6 or later.

To use the Docker image with Singularity, please issue the commands
```
singularity run docker://x11vnc/desktop:master
```

Alternatively, you may use the commands
```
singularity pull --name x11vnc-desktop:master.simg docker://x11vnc/desktop:master
./x11vnc-desktop:master.simg
```

Notes regarding singularity:
- When using Singularity, the user name in the container will be the same
  as that on the host, and the home directory on the host will be mounted
  at /home/$USER by default. You will still have read access to
  /home/$DOCKER_USER.
- To avoid conflict with the user configuration on the host when using
  Singularity, this image uses /bin/zsh as the login shell in the container.
  By default, /home/$DOCKER_USER/.zprofile and /home/$DOCKER_USER/.zshrc
  will be copied to your home directory if they are older than those in
  /home/$DOCKER_USER. In particular, /home/$DOCKER_USER/.profile will be
  sourced by /home/$USER/.zprofile. This works the best if you use another
  login shell (such as /bin/bash) on the host.
- To avoid potential conflict with your X11 configuration, this image uses
  LXQT for the desktop manager. This works best if you do not use LXQT on
  your host.

## License

See the LICENSE file for details.

## Related Projects
 - [novnc/noVNC](https://github.com/novnc/noVNC): VNC client using HTML5 (Web Sockets, Canvas)
 - [fcwu/docker-ubuntu-vnc-desktop](https://github.com/fcwu/docker-ubuntu-vnc-desktop): An original but insecure implementation of Ubuntu desktop, without password protection.
 - [phusion/baseimage](https://github.com/phusion/baseimage-docker): A minimal Ubuntu base image modified for Docker-friendliness
