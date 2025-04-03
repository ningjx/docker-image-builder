FROM debian:bookworm-slim AS builder
USER root
WORKDIR /dotnet
RUN apt-get update && apt-get install -y --no-install-recommends wget && \
    wget -O dotnetsdk.tar.gz --no-check-certificate https://download.visualstudio.microsoft.com/download/pr/9d07577e-f7bc-4d60-838d-f79c50b5c11a/459ef339396783db369e0432d6dc3d7e/dotnet-sdk-8.0.407-linux-x64.tar.gz && \
    tar zxf dotnetsdk.tar.gz -C /dotnet && \
    rm dotnetsdk.tar.gz && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

FROM jenkins/jenkins:lts-jdk21
USER root

COPY --from=builder /dotnet /dotnet
RUN apt-get update && \
    apt-get install -y --no-install-recommends docker.io libc6 libgcc-s1 libgssapi-krb5-2 libicu72 libssl3 libstdc++6 zlib1g && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV TZ=Asia/Shanghai
ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=true
ENV DOTNET_ROOT=/dotnet
ENV PATH=$PATH:/dotnet
ENV JENKINS_UC=https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
ENV JENKINS_UC_EXPERIMENTAL=https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
ENV JENKINS_INCREMENTALS_REPO_MIRROR=https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json
