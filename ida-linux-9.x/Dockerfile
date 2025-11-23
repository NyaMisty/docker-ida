FROM debian:13

USER root

ENV DEBIAN_FRONTEND=noninteractive
ENV UV_PYTHON_INSTALL_DIR=/opt/uv/python
ENV UV_PYTHON_BIN_DIR=/opt/uv/bin
ENV UV_TOOL_DIR=/opt/uv/tools
ENV UV_TOOL_BIN_DIR=/opt/uv/bin
ENV UV_CACHE_DIR=/opt/uv/cache

ENV PATH="/opt/uv/bin:/opt/ida:$PATH"

# Add 32 bit architecture support and deps for IDA
RUN dpkg --add-architecture i386 && apt-get -y update && apt-get -y dist-upgrade

ARG QT=5
RUN apt-get update && \
    ([ $QT -eq 5 ] && apt install -y \
        curl xvfb qtbase5-dev \
    ) || ([ $QT -eq 6 ] && apt install -y \
        curl xvfb qt6-base-dev \
    ) || false

ARG PYTHON_VERSION="3.12"

# Use uv to manage python env
RUN curl -LsSf https://astral.sh/uv/install.sh | sh && \
    cp /root/.local/bin/uv /usr/local/bin/uv && \
    cp /root/.local/bin/uvx /usr/local/bin/uvx && \
    uv python install "${PYTHON_VERSION}" && (cd $UV_PYTHON_BIN_DIR && ln -s python3.* python3 && cp -a python3 python)
# Force install pip to uv's python
RUN rm $(dirname $(uv python find))/../lib/python3.*/EXTERNALLY-MANAGED && \
    python3 -m ensurepip && \
    (cd $UV_PYTHON_BIN_DIR && ln -s "$(dirname $(uv python find))/pip" pip && cp -a pip pip3)

# Change ldconfig to force IDA to use uv's python
RUN echo "$(realpath $(dirname $(uv python find))/../lib)" >> "/etc/ld.so.conf.d/python3.conf" &&  \
    ldconfig

ARG INSTALLER=ida-pro_92_x64linux.run
RUN --mount=type=bind,src=${INSTALLER},dst=/tmp/installer.run \
    mkdir -p /root/.local/share/applications/ && \
    INTERP=$(echo /lib*/ld-*so* | awk '{print $1}') && (printf "\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\ny\n/opt/ida/\n\n\n" | $INTERP /tmp/installer.run)

#ARG TOOL_PACKAGE=ida-pro_90sp1_tools.tar
ARG TOOL_PACKAGE=
RUN --mount=type=bind,src=${TOOL_PACKAGE},dst=/tmp/tools.tar \
    if [ -f /tmp/tools.tar ]; then \
        ls -al /tmp/tools.tar && mkdir -p /opt/ida/tools && cd /opt/ida/tools && tar xvf /tmp/tools.tar; \
    fi


# Run ida license patch
ADD keygen-v2.py /opt/ida
RUN cd /opt/ida && python3 keygen-v2.py --oneshot --name docker-ida

# Setup idalib
RUN cd /opt/ida/idalib/python && pip install . && python3 py-activate-idalib.py

# Bypass EULA Dialog
RUN python3 -c "import idapro; import idaapi; idaapi.reg_write_int('EULA 90', 1)"