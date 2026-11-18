
FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

# Basic dependencies + Java 17
RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip \
    xz-utils \
    zip \
    wget \
    ca-certificates \
    libglu1-mesa \
    openjdk-17-jdk \
    && rm -rf /var/lib/apt/lists/*

# Java environment
ENV JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64"

# Flutter 3.47.4
RUN git clone --branch 3.47.4 \
    https://github.com/flutter/flutter.git \
    /opt/flutter

ENV PATH="/opt/flutter/bin:/opt/flutter/bin/cache/dart-sdk/bin:$PATH"

# Android SDK
ENV ANDROID_HOME="/opt/android-sdk"
ENV ANDROID_SDK_ROOT="/opt/android-sdk"

RUN mkdir -p ${ANDROID_HOME}/cmdline-tools

# Android command-line tools
RUN wget -q \
    "https://dl.google.com/android/repository/commandlinetools-linux-14742923_latest.zip" \
    -O /tmp/cmdline-tools.zip \
    && unzip -q /tmp/cmdline-tools.zip -d /tmp \
    && mv /tmp/cmdline-tools ${ANDROID_HOME}/cmdline-tools/latest \
    && rm /tmp/cmdline-tools.zip

ENV PATH="${ANDROID_HOME}/cmdline-tools/latest/bin:${ANDROID_HOME}/platform-tools:$PATH"

# Accept Android SDK licenses
RUN yes | sdkmanager --licenses > /dev/null || true

# Android SDK components required by Flutter
RUN sdkmanager \
    "platform-tools" \
    "platforms;android-36" \
    "build-tools;35.0.1" \
    "build-tools;28.0.3"

# Configure Flutter
RUN flutter config --android-sdk ${ANDROID_HOME} \
    && flutter config --jdk-dir ${JAVA_HOME}

# Download Flutter Android and Web artifacts
RUN flutter precache --android --web

WORKDIR /workspaces/my-flutter-project
