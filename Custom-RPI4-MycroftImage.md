1. Download Ubuntu Server Minimal Image:
    - Get the latest image from https://github.com/TheRemote/Ubuntu-Server-raspi4-unofficial/releases (https://github.com/TheRemote/Ubuntu-Server-raspi4-unofficial/releases/download/v28/ubuntu-18.04.4-preinstalled-server-arm64+raspi4.img.xz)
    - Use Etcher to make SD Card
    - Boot into Ubuntu Server Minimal
    - Setup Username and Password as "mycroft" and "mycroft"
    - Connect to a network
    
    ```
    sudo apt-get update && sudo apt-get dist-upgrade -y
    ```

2. Install following packages with apt (done in debos):
    ```
    sudo apt install git gnupg wget curl apt-transport-https software-properties-common
    ```
    
3. Add Repositories (done in debos recipe)
    Add bionic-updates apt repo:
    ```
    echo "deb http://ports.ubuntu.com/ubuntu-ports bionic-updates main restricted multiverse universe" >> /etc/apt/sources.list
    echo "deb-src http://ports.ubuntu.com/ubuntu-ports bionic-updates main restricted multiverse universe" >> /etc/apt/sources.list
    ```
    Add bionic-backports apt repo:
    ```
    echo "deb http://ports.ubuntu.com/ubuntu-ports bionic-backports main restricted multiverse universe" >> /etc/apt/sources.list
    echo "deb-src http://ports.ubuntu.com/ubuntu-ports bionic-backports main restricted multiverse universe" >> /etc/apt/sources.list
    ```
    Add KDE Neon gpg key:
    ```
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys '444D ABCF 3667 D028 3F89 4EDD E6D4 7362 5575 1E5D'
    ```
4. Add neon apt repository:
   ```
   echo "deb https://archive.neon.kde.org/unstable bionic main" > /etc/apt/sources.list.d/neon.list
   echo "deb-src https://archive.neon.kde.org/unstable bionic main" > /etc/apt/sources.list.d/neon.list
   ```
   
5. Mycroft-Core Installation:
   
   ##### Install Mycroft-Core:
   ```
   sudo apt-get install git python3 python3-dev python3-setuptools libtool libffi-dev libssl-dev autoconf automake bison swig libglib2.0-dev portaudio19-dev mpg123 screen flac curl libicu-dev pkg-config libjpeg-dev libfann-dev build-essential jq
   cd ~
   git clone https://github.com/MycroftAI/mycroft-core
   cd mycroft-core
   ./dev_setup.sh -sm 
   ```
   
   ##### Install Mimic:
   ```
   wget http://frozenmazegames.se/mimic-arm64_1.2.0.2+1559651054-1.deb
   sudo dpkg -i mimic-arm64_1.2.0.2+1559651054-1.deb
   ````
   
6. Install Plasma Nano, Mycroft-GUI, Network-Manager Bits:

    ##### Install System Packages First:
    ```
    sudo apt install sddm kwin-wayland kwin-x11 openssh-server ftp i2c-tools konsole nano plasma-workspace-wayland plasma-workspace plasma-pa plasma-widgets-addons libkf5wallet-bin gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-fluendo-mp3 qml-module-qtmultimedia network-manager plasma-nm konsole plasma-workspace-dev
    
    # Select SDDM if asked for configuring Login Manager.
    ```
    
    ##### Build Plasma-Nano:
    ```
    sudo apt build-dep plasma-nano
    cd ~
    git clone https://github.com/KDE/plasma-nano
    cd plasma-nano
    mkdir build
    cd build
    cmake .. -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release -DKDE_INSTALL_LIBDIR=lib -DKDE_INSTALL_USE_QT_SYS_PATHS=ON
    make -j 4
    sudo make install
    ```
    
    ##### Install Depenencies For Mycroft-GUI:
    ```
    sudo apt-get install -y git-core g++ cmake extra-cmake-modules kio-dev gettext pkg-config pkg-kde-tools qtbase5-dev qtdeclarative5-dev kio-dev libkf5notifications-data libkf5notifications-dev qml-module-qtquick2 qml-module-qtquick-controls2 qml-module-qtquick-controls qml-module-qtwebsockets qml-module-qt-websockets qtdeclarative5-qtquick2-plugin qtdeclarative5-models-plugin cmake cmake-extras cmake-data qml-module-qtquick-layouts libkf5plasma-dev extra-cmake-modules qtdeclarative5-dev build-essential g++ gettext libqt5webkit5 libqt5webkit5-dev libkf5i18n-data libkf5i18n-dev libkf5i18n5 qml-module-qtgraphicaleffects libqt5dbus5 libkf5dbusaddons-dev libdbus-1-dev libdbus-glib-1-dev kio-dev libkf5kio-dev libqt5websockets5-dev libqt5webview5-dev qml-module-qtquick-virtualkeyboard libqt5virtualkeyboard5-dev qtvirtualkeyboard-plugin
    
    ```
    
    ##### Build Mycroft-GUI:
    ```
    cd ~
    git clone https://github.com/MycroftAI/mycroft-gui/
    cd mycroft-gui
    mkdir build
    cd build
    cmake .. -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release -DKDE_INSTALL_LIBDIR=lib -DKDE_INSTALL_USE_QT_SYS_PATHS=ON
    make -j 4
    sudo make install
    ```
    
    ##### Build Mycroft-GUI-Mark-2:
    ```
    cd ~
    git clone https://github.com/MycroftAi/mycroft-gui-mark-2
    cd mycroft-gui-mark-2
    mkdir build
    cd build
    cmake .. -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release -DKDE_INSTALL_LIBDIR=lib -DKDE_INSTALL_USE_QT_SYS_PATHS=ON
    make -j 4
    sudo make install
    ```
    
    ##### Build QML Lottie:
    ```
    cd ~
    git clone https://github.com/kbroulik/lottie-qml
    cd lottie-qml
    mkdir build
    cd build
    cmake .. -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=Release -DKDE_INSTALL_LIBDIR=lib -DKDE_INSTALL_USE_QT_SYS_PATHS=ON
    make -j 4
    sudo make install
    ```

7. Download Scripts/Overlays from RPI4 for Mycroft:

    Run Following Scripts:

    ```
    cd ~
    git clone https://github.com/MycroftAI/mycroft-devices
    cd mycroft-devices
    cd scripts

    ###### (as done in debos)
    chmod a+x 93-create_ramdisk.sh
    sudo ./93-create_ramdisk.sh

    ###### (as done in debos)
    chmod a+x 94-create_swap.sh
    sudo ./94-create_swap.sh

    ###### (as done in debos)
    chmod a+X 03-setup_locale.sh
    sudo ./03-setup_locale.sh
    
    ```

    - Copy the following folders and files mentioned here https://github.com/MycroftAI/mycroft-devices/tree/master/overlays/base-embedded/etc/xdg to /etc/xdg on image
    
    - Copy the following mentioned here https://github.com/MycroftAI/mycroft-devices/tree/master/overlays/mark2/etc/xdg to /etc/xdg on image
    
    - Copy the following mentioned here https://github.com/MycroftAI/mycroft-devices/tree/master/overlays/mycroft/etc to etc/ on image
    
    - In /etc/sudoers.d on image and change username in all the scripts inside the folder to your created username
    
    - Copy the following mentioned here https://github.com/MycroftAI/mycroft-devices/tree/master/overlays/base-embedded/etc/skel/.local/share/kwalletd to /etc/skel/.local/kwalletd on image

    - Copy the following mentioned here https://github.com/MycroftAI/mycroft-devices/tree/master/overlays/mycroft/etc/sddm.conf.d to /etc/sddm.conf.d on image and change username in all the scripts inside the folder to your created username

    - Copy only the "renderloop.sh" script from the following mentioned here https://github.com/MycroftAI/mycroft-devices/tree/master/overlays/base-embedded/etc/profile.d to /etc/profile.d on image

    - Copy the following mentioned here https://github.com/MycroftAI/mycroft-devices/tree/master/overlays/mycroft/etc/polkit-1/localauthority/50-local.d to /etc/polkit-1/localauthority/50-local.d and change username and parameters in all the scripts inside the folder to your created username

    - Copy the following mentioned here https://github.com/MycroftAI/mycroft-devices/tree/master/overlays/base-embedded/etc/netplan to /etc/netplan on image

    - Copy the following mentioned here https://github.com/MycroftAI/mycroft-devices/tree/master/overlays/mycroft/etc/mycroft to /etc/mycroft on image

8. Enabled V3D Graphics on RPI4
    
    If /opt/vc does not exist on image download https://github.com/raspberrypi/firmware/tree/master/opt/vc folder only to /opt/vc.
    - LD Link VC LIB
    ```
    cd /etc
    cd ld.so.conf.d
    touch vclib.conf
    sudo nano vclib.conf
    ## on the first line add and save:
    /opt/vc/lib
    ```
    
    Install Graphics Driver PPA
    ```
    sudo add-apt-repository ppa:oibaf/graphics-drivers
    sudo apt update
    sudo apt upgrade
    sudo apt install mesa libdrm
    ```
    
    The RPI4 config.txt should look more like this https://github.com/MycroftAI/mycroft-devices/blob/master/overlays/rpi4-config/config.txt
    
9. Reboot To Mycroft
