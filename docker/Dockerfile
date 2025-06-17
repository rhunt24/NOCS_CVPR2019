# To build: docker build --network host -t nocs .
# To run: docker run -it --runtime=nvidia nocs /bin/bash

FROM arm64v8/ubuntu:20.04
 
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get install -y tzdata software-properties-common

RUN add-apt-repository ppa:deadsnakes/ppa && apt-get update
 
# Install Python 3.7
RUN apt-get update && apt-get install -y \
    python3.7 python3.7-dev python3-pip \
    build-essential curl git unzip
 
# Create python -> python3.7 symlink before using it
RUN ln -s /usr/bin/python3.7 /usr/bin/python

RUN apt-get install -y python3.7-distutils
 
# Install pip
RUN python -m pip install --upgrade pip
RUN pip install --upgrade pip setuptools wheel
# ENV HDF5_DIR=/usr/lib/aarch64-linux-gnu
# ENV LD_LIBRARY_PATH=/usr/lib/aarch64-linux-gnu:$LD_LIBRARY_PATH

# h5py
# RUN apt-get update
RUN apt-get install -y libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev liblapack-dev libblas-dev gfortran
RUN apt-get install -y python3-pip
RUN pip3 install -U pip testresources setuptools
RUN ln -s /usr/include/locale.h /usr/include/xlocale.h
RUN pip3 install Cython==0.29.36
RUN pip3 install pkgconfig
WORKDIR .
COPY ./h5py-3.8.0-cp37-cp37m-manylinux_2_17_aarch64.manylinux2014_aarch64.whl .
COPY ./numpy-1.21.6-cp37-cp37m-manylinux_2_17_aarch64.manylinux2014_aarch64.whl .

ENV H5PY_SETUP_REQUIRES=0 
RUN pip install h5py-3.8.0-cp37-cp37m-manylinux_2_17_aarch64.manylinux2014_aarch64.whl --no-deps --no-build-isolation
RUN pip install numpy-1.21.6-cp37-cp37m-manylinux_2_17_aarch64.manylinux2014_aarch64.whl --no-deps --no-build-isolation

RUN apt-get install -y \
    libgl1-mesa-glx \
    libgl1-mesa-dri \
    libgl1 \
    libxext6 \
    libsm6 \
    libxrender1 \
    libglib2.0-0 \
    nano

RUN pip install \
    tensorflow==2.10.0 \
    keras==2.10.0 \
    matplotlib \
    opencv-python==4.5.3.56 \
    Pillow==8.2.0 \
    scikit-image \
    scikit-learn \
    pycocotools \
    open3d \
    scipy

# WORKDIR /workspace
VOLUME ["/nocs_dir"]

WORKDIR /nocs_dir
RUN git clone https://github.com/rhunt24/NOCS_CVPR2019.git

ENV LD_PRELOAD=/usr/lib/aarch64-linux-gnu/libgomp.so.1
 
CMD [ "python" ]