FROM debian:latest


ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

RUN apt-get update --fix-missing && \
    apt-get install -y wget bzip2 ca-certificates curl git openjdk-8-jdk scala vim procps && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-4.5.11-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh && \
    /opt/conda/bin/conda clean -tipsy && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc
RUN pip install --upgrade pip
RUN pip install numpy pandas scikit-learn

WORKDIR /root
RUN wget --quiet http://archive.apache.org/dist/spark/spark-2.4.2/spark-2.4.2-bin-hadoop2.7.tgz 
RUN tar -xf spark-2.4.2-bin-hadoop2.7.tgz -C /opt/ && \
	ln -s /opt/spark-2.4.2-bin-hadoop2.7 /opt/spark && \
	rm spark-2.4.2-bin-hadoop2.7.tgz && \
	echo 'export PATH=$PATH:/opt/spark/bin' >> ~/.bashrc && \
	echo 'export SPARK_HOME=/opt/spark' >> ~/.bashrc


ENV TINI_VERSION v0.16.1
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini
EXPOSE 8080 7077 066

ENTRYPOINT [ "/usr/bin/tini", "--" ]
CMD /opt/spark/sbin/start-slave.sh -c 1 -m 1G spark://spark-master:7077 ; /bin/bash
