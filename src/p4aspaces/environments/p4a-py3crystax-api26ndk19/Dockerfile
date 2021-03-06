
FROM ubuntu

ENV CRYSTAX_FILE="crystax-ndk-10.3.2-linux-x86_64"
ENV SDK_TOOLS="sdk-tools-linux-4333796.zip"
ENV NDKVER=10.3.2
ENV NDKDIR=/crystax-ndk/
ENV NDKAPI=19
ENV ANDROIDAPI=26
ENV PIP=pip3

# Basic image upgrade:
RUN apt update --fix-missing && apt upgrade -y

# Install base packages
RUN apt update && apt install -y zip python3 python-pip python python3-virtualenv python-virtualenv python3-pip curl wget lbzip2 bsdtar && dpkg --add-architecture i386 && apt update && apt install -y build-essential libstdc++6:i386 zlib1g-dev zlib1g:i386 openjdk-8-jdk libncurses5:i386 && apt install -y libtool automake autoconf unzip pkg-config git ant gradle rsync

# Install Android SDK:
RUN mkdir /sdk-install/
RUN cd /sdk-install && wget --read-timeout=5 --tries=0 https://dl.google.com/android/repository/${SDK_TOOLS}
RUN cd /sdk-install && unzip ./sdk-tools-*.zip && chmod +x ./tools//bin/sdkmanager
RUN /sdk-install/tools/bin/sdkmanager --update
RUN yes | /sdk-install/tools/bin/sdkmanager "platform-tools" "platforms;android-$ANDROIDAPI" "ndk-bundle" "build-tools;26.0.1"

# Get CrystaX NDK:
RUN mkdir -p /crystax-ndk && cd /crystax-ndk && wget --read-timeout=5 --tries=0 https://www.crystax.net/download/${CRYSTAX_FILE}.tar.xz -O crystax.tar.xz && tar xf crystax.tar.xz \
    --exclude='crystax-ndk-*/docs'\
    --exclude='crystax-ndk-*/samples'\
    --exclude='crystax-ndk-*/tests'\
    --exclude='crystax-ndk-*/llvm*'\
    --strip-components=1 \
    && rm /crystax-ndk/crystax.tar.xz
RUN ln -s /usr/bin/python3 /usr/bin/python3.5

# Install shared packages:
{INSTALL_SHARED_PACKAGES}

{SETUP_USER_ENV}
