# JEH Metabolomics Dockerfile
# v1.0 2025-01-14
# Define base image
FROM nvidia/cuda:12.6.3-cudnn-devel-ubuntu24.04

# Install sudo and create a sudo user
RUN apt update && apt install -y sudo \
    && useradd -m -s /bin/bash dockeruser \
    && echo 'dockeruser ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Switch to the new user
USER dockeruser

# Set the working directory
WORKDIR /home/dockeruser

# Set the SHELL to bash
SHELL ["/bin/bash", "-c"]

# Install git, curl and wget
RUN sudo apt update
RUN sudo apt install -y git curl wget

# Pull cuda-samples, make deviceQuery.
RUN git clone https://github.com/NVIDIA/cuda-samples.git \
    && cd cuda-samples/Samples/1_Utilities/deviceQuery \
     && make

# Install Miniconda
RUN curl -O https://repo.anaconda.com/miniconda/Miniconda3-py39_24.11.1-0-Linux-x86_64.sh \
     && bash Miniconda3-py39_24.11.1-0-Linux-x86_64.sh -b -p $HOME/miniconda3 \
        && rm Miniconda3-py39_24.11.1-0-Linux-x86_64.sh

# Initialize conda    
RUN ./miniconda3/bin/activate \
      && ./miniconda3/bin/conda init --all

# Add conda to PATH
ENV PATH="/home/dockeruser/miniconda3/bin:/home/dockeruser/miniconda3/condabin:${PATH}"

# Install dependencies (apt)
RUN sudo apt install -y libxt6 libxt-dev libarchive-dev

# Install dependencies (conda)
RUN conda config --add channels bioconda
RUN conda config --add channels conda-forge
# RUN conda config --set channel_priority strict
RUN conda install -y \
    pip \
    r-base >=4.4.0 \
    r-essentials >=4.4 \
    tensorflow-gpu \
    numpy \
    pandas \
    scikit-learn \
    pillow \
    h5py \
    keras \
    r-cairo \
    cairo \
    bioconductor-xcms \
    bioconductor-MsExperiment \
    bioconductor-MetaboCoreUtils \
    bioconductor-MsCoreUtils \
    bioconductor-Spectra \
    rpy2 \
    neatms \
    pymzml \
    jupyterlab

# Install packages (python)
RUN pip install --upgrade pip
RUN pip install git+https://github.com/PangeAI/simms

# Update cudNN
RUN pip install --upgrade nvidia-cudnn-cu12