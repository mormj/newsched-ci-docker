FROM fedora:34
LABEL maintainer="jmorman@gnuradio.org"

ENV security_updates_as_of 2021-11-11

# Build
RUN dnf install --refresh -y \
        cmake \
        gcc \
        gcc-c++ \
# CPP deps
        boost-devel \
        spdlog-devel \
        fmt-devel \
        yaml-cpp-devel \
        gtest-devel \
        cppzmq-devel \
# Math libraries
        fftw-devel \
        gsl-devel \
        gmp-devel \
# Documentation
        doxygen \
        graphviz \
# gr-soapy
        SoapySDR-devel \
# Python deps
        python3-devel \
        python3-pybind11 \
        python3-numpy \
        python3-pip \
# GUI libraries
        xdg-utils \
        qwt-qt5-devel \
        python3-PyQt5 \
        python3-qt5-devel \
# GRC/next
        python3-pyyaml \
        python3-lxml \
        python3-gobject \
	      gtk3 \
        python3-cairo \
        pango \
# Git For VOLK and libad9361 source building
        git \
# For ccaching
        ccache \
# For testing metainfo files
        appstream \
        && dnf clean all

RUN pip install meson ninja mako six jinja2 pyyaml zmq

# Install VOLK
RUN mkdir -p /src/build && \
    git clone --recursive https://github.com/gnuradio/volk.git /src/volk --branch v2.4.1 --depth 1 && \
    cd /src/build && \
    cmake -DCMAKE_BUILD_TYPE=Release ../volk/ && \
    make -j8 && \
    make install && \
    cd / \
    && rm -rf /src/

# Install Flatc
RUN mkdir -p /src/build
RUN git clone https://github.com/google/flatbuffers.git /src/flatbuffers --branch v2.0.0
RUN cd /src/build && cmake -DCMAKE_BUILD_TYPE=Release ../flatbuffers/ && make -j8 && make install && cd / && rm -rf /src/
