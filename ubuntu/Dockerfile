FROM debian:latest

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

RUN apt-get update --fix-missing && \
    apt-get install -y wget bzip2 ca-certificates curl git nano vim && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-4.7.12-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh && \
    /opt/conda/bin/conda clean -tipsy && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc && \
    find /opt/conda/ -follow -type f -name '*.a' -delete && \
    find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
    /opt/conda/bin/conda clean -afy

RUN conda update -n base -c defaults conda

ADD ubuntu/bioinformatics.yml /tmp/environment1.yml
ADD ubuntu/genomics.yml /tmp/environment2.yml

RUN conda env create -n bioinformatics -f /tmp/environment1.yml 
ENV PATH /opt/conda/envs/bioinformatics/bin:$PATH
RUN conda env create -n genomics -f /tmp/environment2.yml 
ENV PATH /opt/conda/envs/genomics/bin:$PATH

RUN conda init bash 

RUN mkdir -p /data
WORKDIR /data

CMD [ "/bin/bash" ]

