
FROM ubuntu

ENV SDK_TOOLS="sdk-tools-linux-4333796.zip"
ENV NDK_DL="https://dl.google.com/android/repository/android-ndk-r17c-linux-x86_64.zip"
ENV NDKDIR=/ndk/
ENV NDKAPI=19
ENV ANDROIDAPI=19
ENV PIP=pip2

# Basic image upgrade:
RUN apt update --fix-missing && apt upgrade -y

# Install base packages
RUN apt update && apt install -y zip python3 python-pip python python3-virtualenv python-virtualenv curl wget lbzip2 bsdtar && dpkg --add-architecture i386 && apt update && apt install -y build-essential libstdc++6:i386 zlib1g-dev zlib1g:i386 openjdk-8-jdk libncurses5:i386 && apt install -y libtool automake autoconf unzip pkg-config git ant gradle rsync

# Install Android SDK:
RUN mkdir /sdk-install/
RUN cd /sdk-install && wget https://dl.google.com/android/repository/${SDK_TOOLS}
RUN cd /sdk-install && unzip ./sdk-tools-*.zip && chmod +x ./tools//bin/sdkmanager
RUN /sdk-install/tools/bin/sdkmanager --update
RUN yes | /sdk-install/tools/bin/sdkmanager "platform-tools" "platforms;android-19" "ndk-bundle" "build-tools;25.0.2"

# Obtain Android NDK:
RUN mkdir -p /tmp/ndk/ && cd /tmp/ndk/ && wget ${NDK_DL} && unzip -q android-ndk*.zip && mv android-*/ /ndk/

# Install shared packages:
{INSTALL_SHARED_PACKAGES}

{SETUP_USER_ENV}
