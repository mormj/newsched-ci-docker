FROM ubuntu:20.04
LABEL maintainer="jmorman@gnuradio.org"

ENV security_updates_as_of 2022-04-15

# Prepare distribution
RUN DEBIAN_FRONTEND=noninteractive \
    apt-get update -q \
    && apt-get -y upgrade

# CPP deps
RUN DEBIAN_FRONTEND=noninteractive \
    apt-get install -qy \
    clang \
    libzmq3-dev \
    doxygen \
    libspdlog-dev \
    libyaml-cpp-dev \
    libgtest-dev \
    libfmt-dev \
    pybind11-dev \
    python3-dev \
    python3-numpy \
    libqwt-qt5-dev \
    qtbase5-dev \
    --no-install-recommends \
    && apt-get clean


# Build deps
RUN mv /sbin/sysctl /sbin/sysctl.orig \
    && ln -sf /bin/true /sbin/sysctl \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    --no-install-recommends \
    build-essential \
    ccache \
    ninja-build \
    libboost-dev \
    libboost-program-options-dev \
    libboost-thread-dev \
    libfftw3-dev \
    libgmp-dev \
    libgsl0-dev \
    doxygen \
    doxygen-latex \
    && rm -f /sbin/sysctl \
    && mv /sbin/sysctl.orig /sbin/sysctl

# Testing deps
RUN DEBIAN_FRONTEND=noninteractive \
    apt-get install -qy \
    xvfb \
    lcov \
    python3-scipy \
    python3-pyqt5 \
    --no-install-recommends \
    && apt-get clean

# Install other dependencies (e.g. VOLK)
RUN apt-get -y install -q \
    git \
    python3-pip \
    cmake \
    pkg-config \
    ca-certificates \
    libsoapysdr-dev \
    libasound2 \
    portaudio19-dev \
    libad9361-0 \
    --no-install-recommends
RUN apt-get clean
RUN apt-get autoclean

RUN pip install meson ninja mako six jinja2 pyyaml zmq jsonschema jsonschema-default packaging

# Install VOLK
RUN mkdir -p /src/build
RUN git clone --recursive https://github.com/gnuradio/volk.git /src/volk --branch v2.4.1
RUN cd /src/build && cmake -DCMAKE_BUILD_TYPE=Release ../volk/ && make && make install && cd / && rm -rf /src/

# Install Flatc
RUN mkdir -p /src/build
RUN git clone https://github.com/google/flatbuffers.git /src/flatbuffers --branch v2.0.0
RUN cd /src/build && cmake -DCMAKE_BUILD_TYPE=Release ../flatbuffers/ && make && make install && cd / && rm -rf /src/


