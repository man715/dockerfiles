# Run Discord in a container
#
# docker run -it \
#	--net host \ # may as well YOLO
#	-v /tmp/.X11-unix:/tmp/.X11-unix \ # mount the X11 socket
#	-e DISPLAY=unix$DISPLAY \
#	--device /dev/snd \ # so we have sound
#   --device /dev/dri \
#	-v /dev/shm:/dev/shm \
#	--name discord \
#	man715/discord
#

# Base docker image
FROM debian:bullseye-slim
LABEL maintainer "Man715 <man715@protonmail.com>"

# Install Discord
Run apt-get update && apt-get install -y \
	apt-transport-https \
	ca-certificates \
	curl \
	gnupg \
	libcanberra-gtk* \
	libgl1-mesa-dri \
	libgl1-mesa-glx \
	libpangox-1.0-0 \
	libpulse0 \
	libv4l-0 \
	--no-install-recommends \
    && curl -p --insecure "http://ftp.de.debian.org/debian/pool/main/liba/libappindicator/libappindicator1_0.4.92-7_amd64.deb" --output libappindicator1_0.4.92-8_amd64.deb \
    && curl -p --insecure "http://ftp.de.debian.org/debian/pool/main/libi/libindicator/libindicator7_0.5.0-4_amd64.deb" --output libindicator7_0.5.0-4_amd64.deb \
    && apt install -y ./libindicator7_0.5.0-4_amd64.deb \
    && apt install -y ./libappindicator1_0.4.92-8_amd64.deb \
    && rm libindicator7_0.5.0-4_amd64.deb \
    && rm libappindicator1_0.4.92-8_amd64.deb

Run curl -p -L "https://discord.com/api/download?platform=linux&format=deb" --output discord.deb \
    && apt install -y ./discord.deb


RUN apt-get purge --auto-remove -y curl \
	&& rm -rf /var/lib/apt/lists/*

RUN groupadd -r discord && useradd -r -g discord -G audio,video discord \
    && mkdir -p /home/discord/Downloads && chown -R discord:discord /home/discord


# Run Discord as non privileged user
USER discord

# Autorun discord
ENTRYPOINT [ "discord" ]
CMD [ "--no-sandbox" ]
