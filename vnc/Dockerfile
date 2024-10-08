FROM marcpartensky/alpine:xorg
LABEL maintainer=<marc.partensky@gmail.com>

USER root
COPY crontab.txt /var/spool/cron/marc
RUN chmod 600 /var/spool/cron/marc

# Install packages
RUN apk add \
    # xorg-server required for xdummy
	xorg-server \
    # xorg-server-utils \
	# xorg-server-xvfb \
	xvfb \
    # xf86-video-dummy \
	tigervnc \
    # sway \
    picom \
    i3wm \
    i3status dmenu pango \
    xterm \
    # weston \
    # weston-backend-drm seatd weston-backend-wayland weston-shell-desktop weston-terminal font-dejavu \
    ncurses \
    ly \
    feh \
    # waybar \
    # swayvnc \
    novnc \
    cronie

RUN chown -R marc:marc /usr/share/novnc
RUN chmod -R 755 /usr/share/novnc
# RUN groupadd supervisor
RUN usermod -aG root marc


# Tested with: x86_64 / aarch64
# ENV ARCH="x86_64" \
#     USER="marc" \
#     VNC_LISTEN_ADDRESS="0.0.0.0" \
#     VNC_AUTH_ENABLE="false" \
#     VNC_PASS="$(pwgen -yns 8 1)"

# Copy sway config
# COPY sway_config /etc/sway/config

# Add wayvnc to compositor startup and put IPC on the network
# RUN mkdir /etc/sway/config.d \
#     && echo "exec wayvnc 0.0.0.0 5900" >> /etc/sway/config.d/exec \
#     && echo "exec \"socat TCP-LISTEN:7023,fork UNIX-CONNECT:/tmp/sway-ipc.sock\"" >> /etc/sway/config.d/exec \
#     && mkdir -p /home/marc/.config/wayvnc/ \
#     && printf "\
# address=$VNC_LISTEN_ADDRESS\n\
# enable_auth=$VNC_AUTH_ENABLE\n\
# username=$USER\n\
# password=$VNC_PASS\n\
# " > /home/$USER/.config/wayvnc/config
# RUN mkdir -p /etc/crontabs/root
COPY crontab.txt /etc/crontabs/root

ENV XDG_RUNTIME_DIR=/run/user/marc
RUN mkdir -p ${XDG_RUNTIME_DIR}
RUN chown -R marc:marc ${XDG_RUNTIME_DIR}

USER marc
EXPOSE 5900 6080
ENV DISPLAY=:0.0
# ENV WAYLAND_DISPLAY=wayland-0
ENV XDG_RUNTIME_DIR=/run/user/marc

RUN echo "#!/bin/sh" > /home/marc/.local/bin/nwpp
RUN echo "feh --bg-scale \$(find wallpapers -type f | shuf -n 1)" >> /home/marc/.local/bin/nwpp
RUN chmod +x /home/marc/.local/bin/nwpp

COPY i3_config /home/marc/.i3/config

COPY entrypoint.sh /tmp/entrypoint.sh
COPY supervisord.conf /etc/supervisord.conf

USER root
ENTRYPOINT ["supervisord", "-l", "/tmp/supervisord.log"]
