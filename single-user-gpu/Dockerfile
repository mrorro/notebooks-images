ARG BASE_IMAGE=marorro/datascience-notebook:latest
# hadolint ignore=DL3006
FROM $BASE_IMAGE

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

RUN apt-get update \
    && apt-get install --no-install-recommends -y \
        less \
        graphviz \
        vim \
        openssh-client \
        libsndfile1 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*


RUN mamba install -y --quiet \
        ipyleaflet \
        jupyterlab==3.1.4 \
        notebook==6.4.1 \
        nbclassic==0.2.8 \
        jupyter-server-proxy \
        jinja2==3.0.3 \
	geopandas \
	ipywidgets \
	matplotlib \
	plotly \
	graphviz \
	jq \
        jupyterlab-git \
        jupyterlab-github \
        mlflow \
        tensorflow
RUN  mamba install pytorch torchvision torchaudio pytorch-cuda=11.6 -c pytorch -c nvidia \
     && conda clean --all

USER $NB_UID

# packages not in conda
RUN python3 -m pip install --no-cache-dir \
        shortid \
        nbgitpuller \
        git+https://github.com/EGI-Foundation/egi-notebooks-addons@0.1.3 \
        h5glance \
        nbtop \
        tflearn \
        jupyter-tensorboard-proxy \
        mlflow-server-proxy 

USER $NB_UID

RUN jupyter labextension install @jupyter-widgets/jupyterlab-manager jupyter-leaflet
# Avoid a broken extension to show up
RUN jupyter labextension disable @pyviz/jupyterlab_pyviz

RUN rmdir work
