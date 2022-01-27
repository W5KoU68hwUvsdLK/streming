FROM ubi8:latest

# 作成したユーザの情報
LABEL maintainer="mitani <mitani@daisuke2.com>"

# RUN: docker buildするときに実行される `
RUN echo "now building..."

## 必要なプログラム類のインストール
RUN yum -y install openssh-server initscripts \
                    unzip \
                    git \
                    glibc \
                    libgcc \
                    libstdc++ \
                    ca-certificates \
                    tar \
                    java-11-openjdk-devel \
                    java-11-openjdk

RUN cd /tmp; curl -L -O https://services.gradle.org/distributions/gradle-7.3.2-bin.zip \
RUN cd /tmp; unzip gradle-7.3.2-bin.zip; mv gradle-7.3.2 gradle; mv gradle /usr/local/

RUN useradd developer && \
    echo "developer:developer" | chpasswd

## SSHの設定
RUN mkdir /var/run/sshd && \
    ssh-keygen -t rsa -N "" -f /etc/ssh/ssh_host_rsa_key && \
    sed -i -e "s/#PermitRootLogin yes/PermitRootLogin yes/g" /etc/ssh/sshd_config && \
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

RUN rm -rf /run/nologin

## 開発環境ディレクトリ作成
RUN mkdir -p /home/developer/src; \
    chown -R developer:developer /home/developer/

# ポート22を開ける
EXPOSE 22 8080

# ENTRYPOINT : docker runするときに実行される
ENTRYPOINT ["/usr/sbin/sshd", "-D"]
