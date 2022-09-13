FROM ubuntu:22.04

ENV USER=dev
ENV PASS=dev
ENV HOME=/home/${USER}

# Install dependencies
RUN apt update -qq -y && apt install -qq -y nano wget sudo

# Install oh-my-zsh
RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.2/zsh-in-docker.sh)" -- \
    -t robbyrussell

# Install python
RUN DEBIAN_FRONTEND=noninteractive apt install -qq -y software-properties-common python3-setuptools && \
    DEBIAN_FRONTEND=noninteractive add-apt-repository ppa:deadsnakes/ppa && \
    DEBIAN_FRONTEND=noninteractive apt install -qq -y python3.9

# Add user
RUN useradd ${USER} && usermod -a -G sudo ${USER}

# Install the armbian builder in users home folder
RUN apt install -qq -y git
RUN cd ${HOME} && git clone --depth 1 https://github.com/armbian/build

# Make user the owner of its home directory and all contents
RUN chown -R ${USER}:${USER} ${HOME}
RUN chmod -R 775 ${HOME}

# Set user pass
RUN echo ${USER}:${PASS} | chpasswd

# Switch to newly created user
USER ${USER}

# Prevent shell slowdown by git_prompt_info
RUN git config --global oh-my-zsh.hide-info 1

# Set workdir to users home
WORKDIR ${HOME}

ENTRYPOINT ["tail", "-f", "/dev/null"]
