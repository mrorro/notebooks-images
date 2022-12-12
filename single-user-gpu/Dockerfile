ARG BASE_IMAGE=marorro/gpu-base:latest
# hadolint ignore=DL3006
FROM $BASE_IMAGE

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN mamba install -y --quiet \
        cftime \
        ipympl \
        jmespath \
        psycopg2 \
        boto3 \
        folium \
        tensorflow \
        tqdm \
        lxml \
        pymongo \
        rasterstats \
        geopandas \
        ipywidgets \
        matplotlib \
        lightgbm \
        eo-learn \
        plotly \
        graphviz \
        jq \
        nb_conda_kernels \
	jupyterlab-git \
        jupyterlab-github \
        mlflow \
    && conda clean --all

RUN mamba install -y --quiet \
        pytorch=1.12.1=py3.10_cuda11.3_cudnn8.3.2_0 torchvision torchaudio cudatoolkit -c pytorch \
    && conda clean --all

# Octave, install on a different environment
# Octave from conda won't build packages
# see https://discourse.jupyter.org/t/installing-octave-packages-with-binder/4206
USER root

RUN apt-get update \
    && apt-get install --install-recommends -y \
        octave octave-signal gnuplot\
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

USER $NB_UID

RUN mamba install -y --quiet octave_kernel=0.32.0 \
    && conda clean --all

# Rstudio
USER root
RUN apt-get update \
    && apt-get install -y gdebi-core \
    && wget -q https://download2.rstudio.org/server/jammy/amd64/rstudio-server-2022.07.2-576-amd64.deb \
    && gdebi -n rstudio-server-2022.07.2-576-amd64.deb \
    && rm rstudio-server-2022.07.2-576-amd64.deb \
    && apt-get remove -y gdebi \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

USER $NB_UID

RUN mamba install -y --quiet \
       -c conda-forge jupyter-rsession-proxy \
    && conda clean --all

# packages not in conda
RUN pip install --no-cache-dir \
        git+https://github.com/EGI-Foundation/egi-notebooks-addons@0.1.3 \
        nbtop \
        panel 

RUN rmdir work
