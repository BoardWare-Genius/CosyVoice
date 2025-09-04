FROM nvidia/cuda:11.8.0-runtime-ubuntu20.04

# 设置 CUDA 环境变量
ENV CUDA_HOME=/usr/local/cuda
ENV PATH=${CUDA_HOME}/bin:${PATH}
ENV LD_LIBRARY_PATH=${CUDA_HOME}/lib64:${LD_LIBRARY_PATH}

# FROM pytorch/pytorch:2.0.1-cuda11.7-cudnn8-runtime
# ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /app
COPY . /app

RUN sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list
RUN apt-get update -y
RUN apt-get -y install git unzip git-lfs g++
RUN git lfs install
# RUN git clone --recursive https://github.com/FunAudioLLM/CosyVoice.git
RUN git submodule update --init --recursive
# here we use python==3.10 because we cannot find an image which have both python3.8 and torch2.0.1-cu118 installed
RUN pip install -r requirements.txt
CMD ["sh", "-c", "cd runtime/python/fastapi && python server.py --port 5000 --model_dir /models/CosyVoice-300M-Instruct"]