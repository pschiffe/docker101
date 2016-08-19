# Linux Containers Course

[![Build Status](https://travis-ci.org/pschiffe/docker101.svg)](https://travis-ci.org/pschiffe/docker101)

Slides are available here - [josefkarasek.github.io/docker101](https://josefkarasek.github.io/docker101/) (source files can be found in the gh-pages branch)

PDF version and archive of the html slides can be found here - [github.com/pschiffe/docker101/releases](https://github.com/pschiffe/docker101/releases)

## Docker Installation
This short tutorial guides you through the installation of Docker. However, it does not second the official documentation. If you happen to miss some information, you'll most likely find it [there](https://docs.docker.com/).

### Fedora / CentOS / RHEL
The Docker service needs some storage considerations. Ideal case is, when your workstation is using LVM for storage, and you have a free space (20GB or more is recommended) in a volume group. Spare partition or disk is also fine. If not, no worries. Docker will still work just fine, with slightly degraded performance, what is perfectly fine for development and testing purposes.

To install Docker on Fedora, run:
```
sudo dnf install docker
```

On CentOS and RHEL, run:
```
sudo yum install docker
```

If you have a free space in your volume group, and you can use it for the Docker storage, modify `/etc/sysconfig/docker-storage-setup` file. For example:
```
VG=my-vol-group
DATA_SIZE=20GB
```

If you have a spare disk or partition, you can use this example:
```
DEVS=/dev/vdb1
VG=docker-vol
DATA_SIZE=95%VG
```

If you haven't, just leave the `/etc/sysconfig/docker-storage-setup` file as it is.

Now just start the Docker service and (optionally) enable it at the boot time:
```
sudo systemctl start docker
sudo systemctl enable docker
```

When running `docker` commands, don't forget to use `sudo`, otherwise you will see something like:
```
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

#### Ubuntu
A section of [this tutorial](https://www.digitalocean.com/community/tutorials/how-to-dockerise-and-deploy-multiple-wordpress-applications-on-ubuntu) can be helpful while installing Docker on Ubuntu.

### Windows or Mac Users
Next couple of lines describe the Atomic Project's bundle, which not only contains Docker, but additional features related to Docker, such as Nulecule or Kubernetes. To install the bundle, you will need these apps (Mac users are encouradged to use brew to [install](#mac_users) them):

1. [VirtualBox](https://www.virtualbox.org/wiki/Downloads) 
2. [Vagrant](https://releases.hashicorp.com/vagrant/1.7.4/vagrant_1.7.4.msi) 1.7.4 - automates operations upon VirtualBox, so you can only run `$ vagrant up` to run the bundle.
3. [Cygwin](https://www.cygwin.com/) - command line terminal to run your commands ([direct download](https://www.cygwin.com/setup-x86_64.exe)). Click through the installation and on the **Choose A Download Site** select **mirrors.kernel.org** as a download site. In the next window (**Select packages**) search for (1) **rsync** and click on the cycling arrows on the **net** line to install this package, then search for (2) **openssh** and do the same to install it.

After the installation restart your PC to apply changes. Then run **CygWin64 Terminal** and type:
```
export VAGRANT_DETECTED_OS=cygwin
mkdir vagrant-images
cd vagrant-images
vagrant init projectatomic/adb
vagrant up
vagrant ssh
```
Now you're running your own installation of the Atomic project's bundle! You can test your docker installation by typing `docker run hello-world`.

## <a name="mac_users"></a>Mac Users
Just use brew to download and install necessary software:
```
brew cask install virtualbox
brew cask install vagrant
brew cask install vagrant-manager
mkdir vagrant-images
cd vagrant-images
vagrant init projectatomic/adb
vagrant up
vagrant ssh
```
If installation via brew fails, use the links above and download installers for Mac.
After successful installation open up terminal and type:
```
mkdir vagrant-images
cd vagrant-images
vagrant init projectatomic/adb
vagrant up
vagrant ssh
```

## Last resort installation
Nor Windows, neither Macs run Docker natively as of yet. To get it there you can use [VirtualBox](https://www.virtualbox.org/wiki/Downloads) and [Docker Toolbox](https://www.docker.com/toolbox). Install VirtualBox first, then Docker Toolbox. After installing both, run the Toolbox. It will download necessary images and load Boot2Docker. Now you can test your installation.

## Test
To ensure you have successfully installed Docker issue following command:
```
docker run hello-world
```

## To pre-download Docker images for workshop, do:
```
docker pull fedora
docker pull nginx
docker pull mariadb
docker pull wordpress
docker pull pschiffe/docker101-gcc
```
