FROM nginx
USER root
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib" > /etc/apt/sources.list
RUN echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib" >> /etc/apt/sources.list
RUN echo "deb https://security.debian.org/debian-security bullseye-security main contrib" >> /etc/apt/sources.list
RUN apt update && apt install -y vim cron socat logrotate
RUN curl https://get.acme.sh | sh -s email="my@example.com"
RUN echo "export PATH="$HOME/.acme.sh:$PATH"" >> ~/.bashrc
RUN /root/.acme.sh/acme.sh --upgrade --auto-upgrade
RUN /root/.acme.sh/acme.sh --set-default-ca --server letsencrypt
RUN echo '#!/bin/bash' > /entry.sh && \
    echo 'echo "export Ali_Key="$Ali_Key"" >> ~/.bashrc' >> /entry.sh && \
    echo 'echo "export Ali_Secret="$Ali_Secret"" >> ~/.bashrc' >> /entry.sh && \
    echo 'if grep -q "CERT_HOME='\''/etc/nginx/ssl'\''" /root/.acme.sh/account.conf; then' >> /entry.sh && \
    echo '    echo "CERT_HOME 已经正确设置为 '\''/etc/nginx/ssl'\''，忽略操作。"' >> /entry.sh && \
    echo 'elif grep -q "CERT_HOME=" /root/.acme.sh/account.conf; then' >> /entry.sh && \
    echo '    sed -i "s|CERT_HOME=.*|CERT_HOME='\''/etc/nginx/ssl'\''|" /root/.acme.sh/account.conf' >> /entry.sh && \
    echo '    echo "CERT_HOME 已修改为 '\''/etc/nginx/ssl'\''。"' >> /entry.sh && \
    echo 'else' >> /entry.sh && \
    echo '    echo "CERT_HOME='\''/etc/nginx/ssl'\''" >> /root/.acme.sh/account.conf' >> /entry.sh && \
    echo '    echo "CERT_HOME 已添加为 '\''/etc/nginx/ssl'\''。"' >> /entry.sh && \
    echo 'fi' >> /entry.sh && \
    echo 'sed -i "s/ACCOUNT_EMAIL='\''my@example.com'\''/ACCOUNT_EMAIL='\''$Email'\''/" /root/.acme.sh/account.conf' >> /entry.sh && \
    echo 'service nginx start' >> /entry.sh && \
    echo 'service cron start' >> /entry.sh && \
    echo 'tail -f /dev/null' >> /entry.sh
RUN chmod +x /entry.sh
ENTRYPOINT ["/entry.sh"]
