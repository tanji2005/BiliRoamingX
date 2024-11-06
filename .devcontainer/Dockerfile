# 使用官方的 OpenJDK 11 镜像作为基础
FROM openjdk:11-jdk

# 设置环境变量
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV PATH=$PATH:$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools

# 安装必要的工具
RUN apt-get update && apt-get install -y --no-install-recommends \
    wget \
    unzip \
    git \
    curl \
    && rm -rf /var/lib/apt/lists/*

# 下载并安装 Android SDK Command-line Tools
RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools && \
    wget https://dl.google.com/android/repository/commandlinetools-linux-8512546_latest.zip -O /tmp/cmdline-tools.zip && \
    unzip /tmp/cmdline-tools.zip -d ${ANDROID_SDK_ROOT}/cmdline-tools && \
    rm /tmp/cmdline-tools.zip && \
    mv ${ANDROID_SDK_ROOT}/cmdline-tools/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/latest

# 同意 Android SDK 许可协议并安装平台工具和构建工具
RUN yes | sdkmanager --sdk_root=${ANDROID_SDK_ROOT} --licenses

RUN sdkmanager --sdk_root=${ANDROID_SDK_ROOT} "platform-tools" "platforms;android-31" "build-tools;31.0.0"

# 设置工作目录
WORKDIR /workspaces/${PROJECT_NAME}

# 默认用户
ARG USERNAME=vscode
RUN usermod -u 1000 ${USERNAME} \
    && groupmod -g 1000 ${USERNAME}

# 复制 gradle wrapper
COPY gradlew /workspaces/${PROJECT_NAME}/
COPY gradle /workspaces/${PROJECT_NAME}/gradle

# 确保 gradlew 可执行
RUN chmod +x /workspaces/${PROJECT_NAME}/gradlew

# 暴露端口（如果需要）
# EXPOSE 8080
