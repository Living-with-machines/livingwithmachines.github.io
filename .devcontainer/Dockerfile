# Start from the Python 3.9
FROM python:3.10-bullseye

# avoid warnings by switching to noninteractive - may turn off later
ENV DEBIAN_FRONTEND=noninteractive

# install python
RUN apt-get update && apt-get install -y \
    python3.10 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

# install GDAL

RUN apt-get update && apt-get install -y \
    libgdal-dev

# install pandoc, pandoc-citeproc, and dependencies for quarto
RUN apt-get update && apt-get install -y --no-install-recommends \
    pandoc \
    pandoc-citeproc \
    curl \
    gdebi-core \
    && rm -rf /var/lib/apt/lists/*

# Download and install Quarto and clean up in the same layer
RUN curl -LO https://quarto.org/download/latest/quarto-linux-amd64.deb \
     && dpkg --add-architecture amd64 \
     && gdebi --non-interactive quarto-linux-amd64.deb \
     && rm -f quarto-linux-amd64.deb

# set correct deno
RUN curl -LO https://deno.land/install.sh && \
    chmod u+x install.sh && \
    sh install.sh -y && \
    rm -f install.sh

ENV QUARTO_DENO=/root/.deno/bin/deno

# install dart-sass

RUN curl -LO https://github.com/sass/dart-sass/releases/download/1.83.4/dart-sass-1.83.4-linux-arm64.tar.gz && \
    tar -xvzf dart-sass-1.83.4-linux-arm64.tar.gz && \
    mv dart-sass /usr/local/bin/sass && \
    rm -f dart-sass-1.83.4-linux-arm64.tar.gz

ENV QUARTO_DART_SASS=/usr/local/bin/sass/sass


# set correct paths

ENV QUARTO_PANDOC=/opt/quarto/bin/tools/x86_64/pandoc
ENV QUARTO_ESBUILD=/opt/quarto/bin/tools/x86_64/esbuild

# export QUARTO_DENO_DOM=$DENO_DOM_PLUGIN    ?
# export QUARTO_SHARE_PATH=$PREFIX/share/quarto    ?

# use pip to install needed packages
RUN pip install --upgrade pip
RUN pip install jupyter nbclient nbformat matplotlib pandas plotly

# install lwm specific requirements (i.e. MapReader) -- ==TODO==
# RUN pip install --no-cache-dir \
#    mapreader@git+https://github.com/Living-with-machines/MapReader.git@main

WORKDIR /workspace

EXPOSE 8888

CMD ["bash"]