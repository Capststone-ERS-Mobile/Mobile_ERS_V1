# # Use Ubuntu as base
# FROM mcr.microsoft.com/devcontainers/javascript-node:20

# # Install dependencies
# RUN apt-get update && \
#     apt-get install -y \
#     openjdk-17-jdk \
#     unzip \
#     wget \
#     curl \
#     git \
#     libgl1-mesa-dev \
#     android-sdk adb

# # Set JAVA_HOME
# ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
# ENV PATH="${JAVA_HOME}/bin:${PATH}"

# # Setup Android SDK
# ENV ANDROID_HOME=/usr/lib/android-sdk
# ENV PATH="$ANDROID_HOME/platform-tools:$ANDROID_HOME/emulator:$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$PATH"

# # Accept licenses
# RUN mkdir -p $ANDROID_HOME && yes | sdkmanager --licenses || true


# Base image with Node.js (v20) and devcontainer tooling
FROM mcr.microsoft.com/devcontainers/javascript-node:20

# Install basic dependencies
RUN apt-get update && \
    apt-get install -y \
    openjdk-17-jdk \
    unzip \
    wget \
    curl \
    git \
    libgl1-mesa-dev \
    lib32stdc++6 \
    lib32z1 \
    watchman \
    adb && \
    apt-get clean

# Set environment variables for Java
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
ENV PATH="${JAVA_HOME}/bin:${PATH}"

# Setup Android SDK
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV PATH="$ANDROID_SDK_ROOT/platform-tools:$ANDROID_SDK_ROOT/emulator:$ANDROID_SDK_ROOT/tools:$ANDROID_SDK_ROOT/tools/bin:$PATH"

# Download and install Android command line tools
RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools && \
    curl -sSL https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip -o tools.zip && \
    unzip tools.zip -d ${ANDROID_SDK_ROOT}/cmdline-tools/tmp && \
    mv ${ANDROID_SDK_ROOT}/cmdline-tools/tmp/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/latest && \
    rm -rf tools.zip ${ANDROID_SDK_ROOT}/cmdline-tools/tmp

# Set sdkmanager to use the installed tools
ENV PATH="${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin:${PATH}"

# Accept Android SDK licenses and install platforms + build tools
RUN yes | sdkmanager --licenses && \
    sdkmanager "platform-tools" "platforms;android-33" "build-tools;33.0.2"

RUN sdkmanager --list || true

# Set permissions for the Android SDK
RUN chmod -R a+rx $ANDROID_SDK_ROOT

# Optional: Set a default working directory
WORKDIR /workspaces/app
