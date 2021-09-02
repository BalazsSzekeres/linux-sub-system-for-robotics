# Linux subsystem for Windows for robotics
## Summary
The aim of this repo to setup a linux distrubution under Windows that can be used for robotics.

## Step by step installation procedure
### Prerequisites
* Windows 10 Insiders preview build 21364 or higher or Windows 11 Build 22000 or higher (required for GUI apps)
* The new Windows Terminal is recommended for ease of use: https://docs.microsoft.com/en-us/windows/terminal/get-started

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
The preferred Ubuntu version can be installed from the Microsoft store. The latest LTS can be found here: https://www.microsoft.com/store/apps/9n6svws3rx71

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
    terminator    
```

