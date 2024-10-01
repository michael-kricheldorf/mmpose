ARG PYTORCH="2.0.0"
ARG CUDA="11.7"
ARG CUDNN="8"

FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

ENV TORCH_CUDA_ARCH_LIST="6.0 6.1 7.0+PTX"
ENV TORCH_NVCC_FLAGS="-Xfatbin -compress-all"
ENV CMAKE_PREFIX_PATH="$(dirname $(which conda))/../"

# To fix GPG key error when running apt-get update
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

RUN apt-get update && apt-get install -y git ninja-build libglib2.0-0 libsm6 libxrender-dev libxext6 libgl1-mesa-glx libgtk2.0-dev pkg-config\
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install xtcocotools
RUN pip install cython
RUN pip install xtcocotools

# Install MMEngine and MMCV
RUN pip install openmim
RUN mim install mmengine "mmcv==2.1.0"

# Install MMPose
RUN conda clean --all
RUN git clone https://github.com/michael-kricheldorf/mmpose.git /mmpose
RUN git clone https://github.com/michael-kricheldorf/roboflow-sports.git /sports
RUN pip install /sports
WORKDIR /mmpose
RUN git checkout main
ENV FORCE_CUDA="1"
#ENV MMCV_WITH_OPS=1
RUN pip install "numpy<2"
RUN pip install "torch>=1.8"
RUN pip install json-tricks
RUN pip install mmdet

# inserted due to an error, from https://stackoverflow.com/questions/67120450/error-2unspecified-error-the-function-is-not-implemented-rebuild-the-libra
#RUN pip install "opencv-python-headless==4.6.0.66"

RUN pip install --no-cache-dir -e .

RUN pip uninstall opencv-python -y

WORKDIR /mmpose/projects/rtmpose3d