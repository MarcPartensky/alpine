FROM alpine:latest
LABEL maintainer="Marc Partensky"

RUN apk update
RUN apk add \
    sudo \
    git \
    zsh \
    shadow \
    neovim \
    stow \
    exa \
    bat \
    make \
    npm \
    go \
    openssh \
    podman \
    podman-compose

RUN useradd --create-home --groups wheel --shell /bin/zsh marc
RUN passwd -d marc
RUN echo "%wheel ALL=(ALL) ALL" >> /etc/sudoers

USER marc

COPY entrypoint.sh /tmp/entrypoint.sh

USER root
# RUN cat /etc/locale.gen
# RUN sed -i -e 's/en_US.UTF-8 UTF-8/fr_FR.UTF-8 UTF-8/' /etc/locale.gen
# RUN cat /etc/locale.gen
# RUN locale-gen
# RUN cat /etc/locale.gen
ENV LC_ALL fr_FR.UTF-8
ENV LANG fr_FR.UTF-8
ENV LANGUAGE fr_FR:fr

USER marc
ENV HOST docker

WORKDIR /home/marc
RUN mkdir git

RUN git clone https://github.com/marcpartensky/dotfiles git/dotfiles
RUN zsh git/dotfiles/main.sh
RUN make -C git/dotfiles stow
RUN touch .profile

# setup shell scripts
RUN git clone https://github.com/marcpartensky/shell git/shell
RUN make -C git/shell
ENV PATH="/home/marc.local/bin:${PATH}"

# RUN git clone https://github.com/marcpartensky/nvim ~/.config/nvim
# RUN make -C ~/.config/nvim setup

ENTRYPOINT [ "/tmp/entrypoint.sh" ]
