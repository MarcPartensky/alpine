FROM marcpartensky/alpine

USER root
RUN apk add \
	net-tools \
	supervisor \
    firefox \
	alacritty \
    # picom \
    # rofi \
    # feh \
    # ttf-meslo-nerd-font-powerlevel10k \
    # for nested x
    xorg-server-xephyr \
    # dwm build dependencies
    libxft \
    libxinerama \
    plasma-desktop

USER root
# ttf-menlo-powerline-git \
# ttf-meslo \
# xorg-xinit \
# xorg-server \
# xorg-server-utils \
# xorg-server-xvfb \

# setup dwn
# RUN git clone https://github.com/marcpartensky/dwm git/dwm
# RUN sed -i 's/#define MODKEY Mod4Mask/#define MODKEY Mod1Mask/' git/dwm/config.h
# RUN sed -i 's/MODKEY,                       XK_d/MODKEY,                     XK_x/' git/dwm/config.h
# RUN make -C git/dwm
# USER root
# RUN make -C git/dwm install
# USER marc

# setup dwmblocks
# RUN git clone https://github.com/marcpartensky/dwmblocks git/dwmblocks
# RUN make -C git/dwmblocks
# USER root
# RUN make -C git/dwmblocks install
# USER marc

# setup wallpapers
RUN git clone https://github.com/marcpartensky/wallpapers wallpapers
RUN ln -sf /usr/bin/supervisorctl /usr/local/bin/s

USER root
COPY entrypoint.sh entrypoint.sh
COPY supervisord.conf /etc/supervisord.conf
ENTRYPOINT [ "./entrypoint.sh" ]
# ENTRYPOINT [ "DISPLAY=:0", "Xephyr", ":1" ]
