# JEH Metabolomics Pipeline

This repository contains a proof of concept Metabolomics Pipeline for the processing and structural assignment of LC-MS/MS (data dependant acquisition) data by modern methods:

    A - [Files](https://github.com/jonathan-hunter/Metabolomics-Pipeline/tree/master/Docker) for the generation of a docker containerised environment, that is GPU enabled (CUDA) and utilises Python and R languages. Key libraries include Tensorflow and Pytorch to support machine learning based methods, and XCMS and PyOpenMS for metabolomics functions.
    
    B - [Jupyter notebooks](https://github.com/jonathan-hunter/Metabolomics-Pipeline/tree/master/Docker/sync) including for the initial exploratory data analysis (EDA) of raw data files with [PyOpenMS](https://github.com/OpenMS/OpenMS/tree/develop/src/pyOpenMS); the extraction of molecular features with [XCMS](https://www.bioconductor.org/packages/release/bioc/html/xcms.html), including deep learning based peak filtering with [NeatMS](https://github.com/bihealth/NeatMS); and the GPU accelerated spectral database searching of target spectra with [SimMS](https://github.com/PangeAI/SimMS).

The Jupyter Notebooks illustrate a worked example and contain all the required code.

Please note that this work is a prototype and should be validated before use.
