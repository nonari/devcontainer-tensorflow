FROM nvidia/cuda:12.3.0-base-ubuntu22.04
LABEL authors="nonari"

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8

COPY setup.sources.sh /setup.sources.sh
COPY setup.packages.sh /setup.packages.sh
COPY gpu.packages.txt /gpu.packages.txt
RUN chmod +x /setup.sources.sh
RUN chmod +x /setup.packages.sh

RUN /setup.sources.sh
RUN /setup.packages.sh /gpu.packages.txt

ARG PYTHON_VERSION=python3.11
#ARG TENSORFLOW_PACKAGE=tf-nightly
ARG TENSORFLOW_PACKAGE=tensorflow

COPY setup.python.sh /setup.python.sh
RUN chmod +x /setup.python.sh
RUN touch /gpu.requirements.txt
RUN /setup.python.sh $PYTHON_VERSION /gpu.requirements.txt
RUN pip install --no-cache-dir "${TENSORFLOW_PACKAGE}==2.13"

COPY setup.cuda.sh /setup.cuda.sh
RUN chmod +x /setup.cuda.sh
RUN /setup.cuda.sh

COPY bashrc /etc/bash.bashrc
RUN chmod a+rwx /etc/bash.bashrc


ENTRYPOINT ["top", "-b"]
