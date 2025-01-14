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

# Install pip
RUN conda install -y pip

# Install packages
RUN pip install --upgrade pip
RUN pip install tensorflow[and-cuda]
RUN pip install pymzml numpy pandas scikit-learn pillow h5py keras NeatMS jupyterlab

# Set the default command to start JupyterLab
CMD ["jupyter", "lab", "--ip=0.0.0.0", "--no-browser"]


