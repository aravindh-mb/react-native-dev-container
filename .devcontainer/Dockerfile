# Dockerfile
FROM node:18-bullseye

# Set environment variables for Android SDK
ENV ANDROID_SDK_ROOT=/usr/local/android-sdk
ENV PATH=$PATH:$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools

# Install required dependencies
RUN apt-get update && apt-get install -y \
    openjdk-17-jdk \
    wget \
    unzip \
    git \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Download and install Android SDK command-line tools
RUN mkdir -p $ANDROID_SDK_ROOT/cmdline-tools \
    && wget https://dl.google.com/android/repository/commandlinetools-linux-9477386_latest.zip -O commandlinetools.zip \
    && unzip commandlinetools.zip -d $ANDROID_SDK_ROOT/cmdline-tools \
    && mv $ANDROID_SDK_ROOT/cmdline-tools/cmdline-tools $ANDROID_SDK_ROOT/cmdline-tools/latest \
    && rm commandlinetools.zip

# Accept Android SDK licenses
RUN yes | sdkmanager --licenses

# Install specific SDK platforms and build tools
RUN sdkmanager \
    "platforms;android-33" \
    "build-tools;33.0.2" \
    "platform-tools" \
    "emulator"

# Set the working directory
WORKDIR /app

# Expose the Metro bundler port
EXPOSE 8081

# Create a non-root user
RUN useradd -m -s /bin/bash node

# Set the user
USER node

# Default command
CMD ["npx", "react-native", "start"]