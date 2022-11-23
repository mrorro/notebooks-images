ARG BASE_IMAGE=eginotebooks/base:latest
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
        jupyterlab-git \
        jupyterlab-github \
        mlflow \
    && mamba install -y --quiet fastai -c fastai \
    && mamba install pytorch torchvision torchaudio pytorch-cuda=11.6 -c pytorch -c nvidia \
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
    && SITE_PACKAGES=$(python -c 'import site; print(site.getsitepackages().pop())') \
    && jupyter kernelspec install "$SITE_PACKAGES/octave_kernel" --name octave --prefix /opt/conda \
    && conda clean --all

# packages not in conda
RUN python3 -m pip install --no-cache-dir \
        git+https://github.com/EGI-Foundation/egi-notebooks-addons@0.1.3 \
        h5glance \
        nbtop \
        tflearn \
        jupyter-tensorboard-proxy \
        mlflow-server-proxy \
        tensorflow

USER root

RUN apt-get update \
    && apt-get install --no-install-recommends -y \
        libsndfile1 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

USER $NB_UID

# Avoid a broken extension to show up
RUN jupyter labextension disable @pyviz/jupyterlab_pyviz

RUN rmdir work
