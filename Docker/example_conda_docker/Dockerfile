FROM continuumio/miniconda3

COPY environment.yml /opt 

RUN conda env create -f /opt/environment.yml

ENTRYPOINT ["conda", "run", "-n", "hisat2"]
