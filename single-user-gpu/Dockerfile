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
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN curl https://raw.githubusercontent.com/kadwanev/retry/master/retry \
         -o /usr/local/bin/retry && chmod +x /usr/local/bin/retry

USER $NB_UID

RUN conda install mamba -y --quiet -c conda-forge \
    && mamba install -y -q \
        r-plotly \
        r-gdata \
        rdflib \
        prov \
        graphviz \
        pydot \
        lxml \
        geojson \
        pydap \
        xarray \
        cartopy \
        ipyleaflet \
        jupyterlab==3.1.4 \
        notebook==6.4.1 \
        nbclassic==0.2.8 \
        jupyter-server-proxy \
        jinja2==3.0.3 \
        jupyterlab-git \
	jupyterlab-github \
	mlflow \
    &&  mamba install pytorch==1.12.1=py3.8_cuda11.3_cudnn8.3.2_0 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=11.3 -c pytorch \
    && conda clean --all

# packages not in conda
RUN pip install --no-cache-dir \
        shortid \
        nbgitpuller \
        jupyter-tensorboard-proxy \
        mlflow-server-proxy \
        tensorflow

WORKDIR "${HOME}"

RUN NODE_OPTIONS="--openssl-legacy-provider" \
    jupyter labextension install \
        @jupyter-widgets/jupyterlab-manager jupyter-leaflet
