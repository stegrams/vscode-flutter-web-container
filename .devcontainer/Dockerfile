FROM ubuntu:20.04

# install all dependencies
ENV DEBIAN_FRONTEND="noninteractive"
RUN apt-get update \
  && apt-get install --yes --no-install-recommends \
     ca-certificates git bash sudo curl file \
     unzip xz-utils zip libglu1-mesa \
  && rm -rf /var/lib/{apt,dpkg,cache,log}

# create user
ARG USER=pick-a-user-name-on-devcontainer-json
RUN useradd -s /bin/bash -m $USER \
  && echo $USER ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USER \
  && chmod 0440 /etc/sudoers.d/$USER

USER $USER
WORKDIR /home/$USER

# flutter
ARG FLUTTER_REL="2.2.0"

ENV FLUTTER_REPO="https://github.com/flutter/flutter"
ENV FLUTTER_HOME="/home/$USER/flutter"
ENV PATH="$FLUTTER_HOME/bin:$PATH"

# git options explained
# --config advice.detachedHead=false: Don't show detached head warning.
# --depth 1: Clone only last commit on each branch.
# ----no-single-branch: Get all channels so I can switch.
# --branch: When cloned, point to the selected branch.
RUN  mkdir -p $FLUTTER_HOME \
  && git clone --config advice.detachedHead=false --depth 1 \
    --no-single-branch --branch $FLUTTER_REL $FLUTTER_REPO $FLUTTER_HOME \
  && flutter config --no-analytics --enable-web

ENTRYPOINT ["flutter", "doctor"]