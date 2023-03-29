FROM fedora:35
LABEL maintainer="jmorman@gnuradio.org"

ENV security_updates_as_of 2022-07-17

# Build
RUN dnf install --refresh -y \
        cmake \
        gcc \
        gcc-c++ \
        clang \
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
        volk-devel \
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
# Audio, SDL
        SDL-devel \
        alsa-lib-devel \
        portaudio-devel \
        jack-audio-connection-kit-devel \
        libsndfile-devel \
# Git For VOLK and libad9361 source building
        git \
# For ccaching
        ccache \
# For testing metainfo files
        appstream \
        && dnf clean all

RUN pip install meson ninja mako six jinja2 pyyaml zmq jsonschema jsonschema-default packaging

# Install Flatc
RUN mkdir -p /src/build
RUN git clone https://github.com/google/flatbuffers.git /src/flatbuffers --branch v2.0.0
RUN cd /src/build && cmake -DCMAKE_BUILD_TYPE=Release ../flatbuffers/ && make -j8 && make install && cd / && rm -rf /src/
