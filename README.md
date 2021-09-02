# Linux subsystem for Windows for robotics
## Summary
The aim of this repo to setup a linux distrubution under Windows that can be used for robotics.

## Step by step installation procedure
### Prerequisites
* Windows 10 Insiders preview build 21364 or higher or Windows 11 Build 22000 or higher (required for GUI apps)
* The new Windows Terminal is recommended for ease of use: https://docs.microsoft.com/en-us/windows/terminal/get-started
* For nvidia GPUs the WSL driver: https://developer.nvidia.com/cuda/wsl/download

### Installing WSL
Run `wsl --install` in the command line. The --install command performs the following actions:
* Enables the optional WSL and Virtual Machine Platform components
* Downloads and installs the latest Linux kernel
* Sets WSL 2 as the default
* Downloads and installs a Linux distribution (reboot may be required)

On an existing WSL setup use: 
1. Use `wsl --update` to update the WSL version.
2. Type `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart` to enable the vm feature.
3. Download the Linux kernel update package: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
4. Then `wsl --set-default-version 2` to set WSL 2 as default version.
5. Followed by `wsl --shutdown`.

### Installing Ubuntu
The preferred Ubuntu version can be installed from the Microsoft store. Ubuntu 18.04 LTS is confirmed working. Kernel versions above 5 can cause unexpected issues, see: https://github.com/hpcng/singularity/issues/5144

After installation, check that the distro is using WSL 2 by `wsl --list --verbose`. If the version is not correct, `wsl --set-version <distribution name> <versionNumber>` can be used to change the version number.

### Installing the required packages
The following command can be used to install every required package:
```
sudo apt-get update && sudo apt-get install -y \
    build-essential \
    libssl-dev \
    uuid-dev \
    libgpgme11-dev \
    squashfs-tools \
    libseccomp-dev \
    wget \
    pkg-config \
    git \
    gitk \
    cryptsetup \
    apt-transport-https \
    apt-xapian-index \
    cmake \
    less \
    mesa-utils\
    nano \
    screen \
    synaptic \
    terminator \
    python \
    dh-autoreconf \
    libarchive-dev
```

If the installation is successfull, the command `glxinfo | grep -i opengl` should return something similiar:
```
OpenGL vendor string: Microsoft Corporation
OpenGL renderer string: D3D12 (Intel(R) UHD Graphics 630)
OpenGL core profile version string: 3.3 (Core Profile) Mesa 21.0.3
OpenGL core profile shading language version string: 3.30
OpenGL core profile context flags: (none)
OpenGL core profile profile mask: core profile
OpenGL core profile extensions:
OpenGL version string: 3.1 Mesa 21.0.3
OpenGL shading language version string: 1.40
OpenGL context flags: (none)
OpenGL extensions:
OpenGL ES profile version string: OpenGL ES 3.0 Mesa 21.0.3
OpenGL ES profile shading language version string: OpenGL ES GLSL ES 3.00
OpenGL ES profile extensions:
```

### Compiling and installing Singularity 2.6.1
1. In the home direcotry create a folder where the source can be downloaded eg: `mkdir singularity-source`
2. In the newly created folder clone the singularity repo: `git clone https://github.com/sylabs/singularity.git`
3. Navigate to the cloned folder and fetch: `git fetch --all`
4. Checkout the tag: `git checkout 2.6.1`
5. Run `./autogen.sh` followed by `./configure --prefix=/usr/local --sysconfdir=/etc` then `make`
6. Finally run `sudo make install`

Singularity will be installed in the `/usr/local` directory hierarchy by default. And if you specify a custom directory with the `--prefix` option, all of Singularity’s binaries and the configuration file will be installed within that directory. This last option can be useful if you want to install multiple versions of Singularity, install Singularity on a shared system, or if you want to remove Singularity easily after installing it.

If you omit the `--sysconfdir` option , the configuration file will be installed in `/usr/local/etc`. If you omit the `--prefix` option, Singularity will be installed in the `/usr/local` directory hierarchy by default. And if you specify a custom directory with the `--prefix` option, all of Singularity’s binaries and the configuration file will be installed within that directory.
