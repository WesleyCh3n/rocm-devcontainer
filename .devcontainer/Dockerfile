FROM rocm/dev-ubuntu-24.04:6.3.4-complete

SHELL ["/usr/bin/bash", "-c"]

RUN apt-get update && \
  apt-get install --no-install-recommends -y git cmake python3-venv python3-dev && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

ARG USERNAME
ARG TZ
ARG VIDEO_GID
ARG RENDER_GID
ARG GPU_ARCH

ENV AMDGPU_TARGETS=${GPU_ARCH}
ENV TZ=${TZ}

# add user and map gpu group
RUN userdel ubuntu \
  && adduser --disabled-password --gecos '' $USERNAME \
  && usermod -aG sudo $USERNAME \
  && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
  && if [[ "$VIDEO_GID" == "$(getent group video | cut -d: -f3)" ]]; then \
    usermod -aG video $USERNAME; \
  else \
    groupadd -g $VIDEO_GID video_map && usermod -aG video_map $USERNAME; \
  fi \
  && if [[ "$RENDER_GID" == "$(getent group render | cut -d: -f3)" ]]; then \
    usermod -aG render $USERNAME; \
  else \
    groupadd -g $RENDER_GID render_map && usermod -aG render_map $USERNAME; \
  fi

USER $USERNAME
WORKDIR /home/$USERNAME

COPY --from=ghcr.io/astral-sh/uv:0.7.8 /uv /uvx /bin/
RUN pip install torch torchvision torchaudio --break-system-packages --index-url https://download.pytorch.org/whl/rocm6.3/ \
  && pip cache purge

ENV PATH="$PATH:/home/$USERNAME/.local/bin"
