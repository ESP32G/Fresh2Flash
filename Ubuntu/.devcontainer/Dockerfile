FROM espressif/idf:release-v5.0

ARG DEBIAN_FRONTEND=nointeractive

ARG CONTAINER_USER=esp32
ARG USER_UID=1000                # first normal user, UID for root is 0
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $CONTAINER_USER \
    && adduser --uid $USER_UID --gid $USER_GID --disabled-password --gecos "" ${CONTAINER_USER} \
    && usermod -a -G dialout $CONTAINER_USER

RUN apt-get update

RUN apt-get -y -q install  cmake git libglib2.0-0 libnuma1 libpixman-1-0 linux-tools-virtual nano python3-pip \
	&& apt-get clean && rm -rf /var/lib/apt/lists/*

RUN update-alternatives --install /usr/local/bin/usbip usbip `ls /usr/lib/linux-tools/*/usbip | tail -n1` 20

USER ${CONTAINER_USER}
ENV USER=${CONTAINER_USER}
WORKDIR /home/${CONTAINER_USER}

RUN echo "source /opt/esp/idf/export.sh > /dev/null 2>&1" >> ~/.bashrc

RUN /opt/esp/entrypoint.sh && pip install --no-cache-dir idf-component-manager

CMD ["/bin/bash"]
