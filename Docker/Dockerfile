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
RUN sudo apt install -y git curl wget pip python3-full


# Pull cuda-samples, make deviceQuery.
RUN git clone https://github.com/NVIDIA/cuda-samples.git \
    && cd cuda-samples/Samples/1_Utilities/deviceQuery \
     && make

# Install python and create a virtual environment     
RUN sudo apt install -y python3-full
RUN python3 -m venv ~/venv/

# Install packages (python)
RUN ~/venv/bin/pip install --upgrade pip
RUN ~/venv/bin/pip install jupyterlab

# Install R
RUN sudo apt install -y r-base r-base-dev

# Install rpy2
RUN sudo apt install -y libtirpc-dev
RUN ~/venv/bin/pip install rpy2

# Install packages contd. (python)
RUN ~/venv/bin/pip install pymzml numpy pandas scikit-learn pillow h5py keras NeatMS matplotlib
RUN ~/venv/bin/pip install git+https://github.com/PangeAI/simms
RUN ~/venv/bin/pip install --upgrade tensorflow[and-cuda]
RUN ~/venv/bin/pip install torch torchvision torchaudio

# Install R packages
RUN sudo chown -R dockeruser /usr/local/lib/R/ && sudo chmod -R 755 /usr/local/lib/R/
RUN sudo chown -R dockeruser /usr/lib/R/ && sudo chmod -R 755 /usr/lib/R/
RUN sudo chown -R dockeruser /usr/share/R/ && sudo chmod -R 755 /usr/share/R/
RUN sudo apt install -y libcurl4-openssl-dev libssl-dev libnetcdf-dev
RUN R -e "options(repos = c(CRAN = 'https://cloud.r-project.org/')); if (!require('BiocManager', quietly = TRUE)) install.packages('BiocManager'); BiocManager::install"
RUN R -e "BiocManager::install(c('xcms', 'MsExperiment', 'MetaboCoreUtils', 'MsCoreUtils', 'Spectra', 'CAMERA', 'MsBackendMgf'))"
RUN R -e "options(repos = c(CRAN = 'https://cloud.r-project.org/')); install.packages('rgl')"


# Update cudNN
RUN ~/venv/bin/pip install --upgrade nvidia-cudnn-cu12

# Install packages contd. (python)
RUN ~/venv/bin/pip install pyteomics lxml sqlalchemy pynumpress spectrum_utils seaborn pyopenms matchms
