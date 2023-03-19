FROM alpine:latest

RUN apk update --no-cache && \
    apk upgrade --no-cache && \
    apk add --no-cache \ 
    bash \ 
    # locales ※無い \ 
    openssh-server \
    sudo \
    supervisor \
    xfce4 \
    xrdp

ADD bin /usr/bin
ADD etc /etc
ADD autostart /etc/xdg/autostart
# #ADD pulse /usr/lib/pulse-10.0/modules/

# Configure
# mkdir /var/run/dbus && \ ※既にフォルダが存在する
RUN cp /etc/X11/xrdp/xorg.conf /etc/X11 && \
  sed -i "s/console/anybody/g" /etc/X11/Xwrapper.config && \
  sed -i "s/xrdp\/xorg/xorg/g" /etc/xrdp/sesman.ini && \
  locale-gen en_US.UTF-8 && \
  echo "xfce4-session" > /etc/skel/.Xclients && \
  cp -r /etc/ssh /ssh_orig && \
  rm -rf /etc/ssh/* && \
  rm -rf /etc/xrdp/rsakeys.ini /etc/xrdp/*.pem

# Docker config
VOLUME ["/etc/ssh","/home"]
EXPOSE 3389 22 9001
ENTRYPOINT ["/usr/bin/docker-entrypoint.sh"]
CMD ["supervisord"]

RUN chmod u+x /usr/bin/docker-entrypoint.sh
