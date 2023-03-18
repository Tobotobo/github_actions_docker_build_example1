FROM alpine:latest

RUN apk update && apk upgrade && \
    apk add --no-cache bash supervisor xfce4 xrdp

# startwm.shファイルを編集して自動ログインを設定する
RUN sed -i '/TerminalServerUsers/d' /etc/xrdp/sesman.ini && \
    sed -i '/TerminalServerAdmins/d' /etc/xrdp/sesman.ini && \
    echo 'xfce4-session' > /etc/skel/.Xclients && \
    echo 'xfce4-session' > /etc/xrdp/startwm.sh && \
    echo 'export GNOME_SHELL_SESSION_MODE=xfce' >> /etc/profile && \
    adduser -D xrdp && \
    echo "xrdp:xrdp" | chpasswd

# supervisord.confファイルをコピーする
COPY supervisord.conf /etc/supervisord.conf

# ポートを公開する
EXPOSE 3389

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
