FROM continuumio/anaconda3:latest

ENV DEBIAN_FRONTEND=noninteractive

# Create a non-root user
RUN groupadd --gid 1000 vscode \
    && useradd --uid 1000 --gid vscode --shell /bin/bash --create-home vscode

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    vim \
    nano \
    build-essential \
    sudo \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Give vscode user sudo privileges
RUN echo "vscode ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Change ownership of conda to vscode user
RUN chown -R vscode:vscode /opt/conda

# Switch to vscode user
USER vscode
WORKDIR /home/vscode

# Update conda and install additional packages
RUN conda update -n base -c defaults conda && \
    conda install -y \
    jupyter \
    jupyterlab \
    pandas \
    numpy \
    matplotlib \
    seaborn \
    scikit-learn \
    requests \
    flask \
    fastapi \
    uvicorn \
    pytest \
    black \
    flake8 \
    ipykernel \
    && conda clean -all

# Install additional pip packages
RUN pip install \
    jupyter-contrib-nbextensions \
    jupyter-nbextensions-configurator \
    plotly \
    dash \
    streamlit

# Configure Jupyter
RUN jupyter notebook --generate-config && \
    echo "c.NotebookApp.ip = '0.0.0.0'" >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.open_browser = False" >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.port = 8888" >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.allow_root = True" >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.token = ''" >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.password = ''" >> ~/.jupyter/jupyter_notebook_config.py

# Set the default working directory
WORKDIR /workspace

# Initialize conda for bash
RUN conda init bash

# Default command
CMD ["/bin/bash"]
